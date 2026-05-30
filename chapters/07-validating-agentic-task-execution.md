> **Voice status:** `voice-unanchored`. Root `style/` and book-level `style/` empty as of this draft.

---

# Chapter 7 — Validating Agentic Task Execution

*A correct final state does not certify a safe path; for agents, the output is the trajectory*

**Author:** Nik Bear Brown
**Editor:** Nik Bear Brown

---

## Suggested titles

1. **Validating Agentic Task Execution: Judge the Trajectory, Not the Endpoint**
2. **Right Answer, Wrong Path: Why Agent Validation Lives in the Sequence of Actions**
3. **Checkpoint Before Irreversibility: Validation in a World With Side Effects**

---

## TL;DR

For every other output type in this book, the thing you validate sits still while you check it — a diff, a claim, a JSON record. An agent's output does not sit still: it is a *sequence of actions with side effects*, executed against a live world, and by the time you see the final state the agent has already clicked, written, deleted, deployed, or paid. So the unit of validation changes. **You validate the trajectory — the ordered sequence of (state, action, observation) steps — not just the endpoint.** This matters because a correct final state can be reached through a broken path (delete-and-recreate, a ten-iteration loop that happened to converge) and a wrong final state hides *where* the agent went off the rails. AgentBench (Liu et al. 2023, arXiv:2308.03688) made this measurable and found the bottleneck is exactly long-horizon execution: poor long-term reasoning, decision-making, and instruction-following — failures that *accumulate over steps*. WebArena (Zhou et al. 2023, arXiv:2307.13854) scores functional task completion in reproducible web environments. Automatic trajectory judges — LLM, VLM, or Agent-as-a-Judge (Zhuge et al. 2024, arXiv:2410.10934) — work *moderately* well: AgentRewardBench (Lù et al. 2025, arXiv:2504.08942) calibrates them at 89.3% inter-annotator agreement, ~80.6% for the best LLM judge versus a rule-based evaluator, ~82–93% for VLM methods — and crucially, **agreement degrades as the trajectory lengthens**, exactly where agents are most dangerous. The one *hard* guarantee is the **checkpoint before an irreversible action**: you cannot reject a deletion after it executes, so you gate it before. And the honest limit: benchmark success does not certify production reliability — a CLEAR-framework study (arXiv:2511.14136) reports agent performance collapsing from **60% on a single run to 25% under 8-run consistency**. `[verify — fresh Nov-2025 preprint; treat as early-stage.]`

---

## Learning objectives

By the end of this chapter you should be able to:

1. **(Understand)** Explain why final-output-only evaluation is insufficient for agents, and define the trajectory as the unit of validation.
2. **(Evaluate)** Diagnose a trajectory for the four named failure modes — loops, scope violations, wrong-tool, error propagation — and say which check (deterministic vs. judge) catches each.
3. **(Evaluate)** Distinguish failures a deterministic gate can catch from those that require a fallible trajectory judge, and calibrate trust using the AgentRewardBench figures and their length-degradation.
4. **(Create)** Place a checkpoint before every irreversible action in a given agent design, applying the irreversibility-gate rule without over-gating into approval fatigue.
5. **(Evaluate)** Read an agent "success rate" and decide whether it reflects single-run benchmark optimism or repeated-run production reliability (the 60%→25% collapse).

## Prerequisites

Ch. 2 (The Deterministic Floor) — the checkpoint-before-irreversibility rule is the deterministic-first principle applied to a world with side effects. Ch. 5 (Validating Reasoning Chains) — a trajectory is a reasoning chain *with actions and observations attached*; the step-level/process-vs-outcome distinction carries over. Ch. 6 (Validating Structured Output) — tool calls are structured output; a wrong-tool or malformed-argument failure is a structured-output failure inside a trajectory. This chapter is the deep version of the coding-agents companion book's review/CI chapters.

---

## 7.1 The agent that deleted the table to fix one row

The following is a composite, assembled from documented agentic failure patterns and labeled as such. The mechanism is real — every failure below is reproduced in the cited literature or in standard agent behavior; the specific team is not.

A platform team gave a database-maintenance agent a narrow task: *one customer's address row has a malformed postal code; correct it.* The agent had read/write access to the customer table and a small toolset — `query`, `update`, `delete`, `create_table`. They evaluated it the way they evaluated everything else: did the final state match the expected state? They ran it, queried the row afterward, and the postal code was correct. Green check. The agent shipped to the nightly maintenance queue.

What the final-state check could not see was the path. The agent had issued the `update`, gotten a transient lock error, decided the table was "corrupted," dropped the entire customer table with `delete`, recreated it with `create_table` from a stale in-context snapshot, and re-inserted the rows — including the corrected one. The final state of *that one row* was correct. The final state of the table was a silent loss of every row written since the snapshot, and a schema subtly different from the original. The endpoint check passed. The trajectory was a catastrophe.

A second run showed a different pathology. Given a row that genuinely could not be updated (a foreign-key constraint), the agent looped: `query` → `update` → error → `query` → `update` → error, eleven times, before a retry happened to coincide with the lock clearing. Final state: correct. Trajectory: eleven writes, eleven errors, a cost spike, and pure luck that it converged at all. A third run called `create_table` when it meant `update` — a wrong-tool selection that, on a less forgiving schema, would have been unrecoverable.

The team made no endpoint error. They made a **validation** error, and it is the one this chapter exists to dislodge: **for an agent, the output is not the final state — it is the trajectory.** A correct endpoint reached by deleting and recreating a table is not a success; it is a near-miss that the endpoint check is structurally blind to. The thesis of this book bites hardest here. **Validation works where ground truth is mechanically available** — you can deterministically detect a loop, a call to a tool outside the allow-list, an error fed into the next step — **and fails where it isn't** — whether the *overall plan* was sound often has no oracle at all, only a fallible judge. And because actions have side effects, there is a category of failure you cannot validate *after*: once the `delete` executes, there is nothing left to reject.

---

## 7.2 The trajectory is the unit: a taxonomy of path failures

Make the trajectory concrete. A trajectory is the ordered sequence of steps an agent takes:

```
step_1: (state_0, action_1, observation_1)
step_2: (state_1, action_2, observation_2)
...
step_n: (state_{n-1}, action_n, observation_n)  ->  final_state
```

Final-output evaluation looks only at `final_state`. Trajectory evaluation looks at the whole chain. AgentBench (Liu et al. 2023) is the empirical reason this distinction matters: across eight distinct interactive environments, the dominant obstacles to usable agents were **poor long-term reasoning, decision-making, and instruction-following** — failures that are invisible at any single step and only emerge as the chain lengthens. The agent does not fail by producing one bad token; it fails by *drifting* over a horizon. WebArena (Zhou et al. 2023) built the reproducible counterpart — fully functional e-commerce, forum, software-dev, and CMS sites — and scored *functional correctness of task completion*, which is itself a trajectory property: did the right things happen, in a working order, to reach a working end?

Four path failures recur, and each maps to a different validation mechanism. Hold the distinction — *which are deterministically catchable, which need a judge* — because it is the whole engineering decision.

| Failure mode | What it looks like in the trajectory | Catchable by | Why |
|---|---|---|---|
| **Loop** | The same (action, observation) cycle repeats: step 4 jumps back to step 2's state | **Deterministic** | State/action-hash repetition is mechanically detectable; set a max-iteration and a cycle detector. No judgment needed. |
| **Scope violation** | An action outside the agent's mandate — a tool not on the allow-list, a resource outside the permitted set | **Deterministic** *if scope is specified* | An allow-list check is a lookup. The hard part is *defining* scope crisply (see misconception below), not enforcing it. |
| **Wrong-tool** | A call to a plausible-but-incorrect tool (`create_table` for an `update`) | **Mixed** | Malformed arguments are deterministic (schema check, Ch. 6); *semantically* wrong tool choice on valid arguments often needs a judge. |
| **Error propagation** | A bad step's output silently feeds the next step as if valid | **Mixed** | Detectable when the error is typed/raised; invisible when a wrong-but-well-formed value flows downstream — that needs a judge or a consistency check. |

The loop and the explicit scope violation are the deterministic floor of agentic validation: a cycle detector and an allow-list catch them with zero judgment, and you should always run both. Wrong-tool and silent error propagation straddle the line — the *shape* of the call is checkable (it is structured output), but whether the *choice* was correct given the goal frequently has no mechanical oracle. That is where the trajectory judge enters, fallible, in §7.4.

> **Misconception to dislodge.** "If the final state is correct, the agent succeeded." False for anything with side effects. The §7.1 agent reached the correct row through a table drop; the looping agent reached it through eleven writes and luck. A correct endpoint says nothing about whether the path was safe, in-scope, reproducible, or even non-destructive. The output *is* the path.

> **Misconception to dislodge.** "Scope violations are easy — just check an allow-list." Enforcing a *specified* scope is easy. *Specifying* it is not. The boundary between an agent helpfully doing a little extra (fetching a related record to complete the task) and an agent exceeding its mandate (modifying that record) is not crisply defined, and a too-tight allow-list breaks legitimate autonomy while a too-loose one lets the table-drop through. Scope is an unsolved definitional problem wearing the costume of a solved lookup problem.

---

## 7.3 The checkpoint before irreversibility: the one hard guarantee

Everything else in this chapter is statistical. This part is not. There is a category of action you cannot validate after the fact, because validating-after means rejecting-after, and you cannot un-delete a table, un-send an email, un-deploy a release, or un-charge a card. For these, the only sound validation is a **checkpoint before execution** — a gate, human or deterministic, placed *before* the irreversible action, where there is still something to reject.

This is the deterministic-first principle of Ch. 2 applied to a world with side effects. After-the-fact validation assumes the output is inert until you accept it; an agent's actions are *not* inert — they fire on execution. So the validation has to move earlier in time, to the last moment a "no" still means something.

```python
IRREVERSIBLE = {"delete", "deploy", "send_email", "charge_card", "drop_table"}

def execute_step(action, args, approve_fn):
    if action in IRREVERSIBLE:
        # validate-before: there is nothing left to reject after this fires
        if not approve_fn(action, args):           # human or deterministic gate
            raise PermissionError(f"checkpoint denied: {action}({args})")
    return tool_dispatch(action, args)
```

The checkpoint is where the chapter's intellectual lineage lives. **Norbert Wiener**'s cybernetics (1948) is the founding idea: a system that acts in the world must continuously compare its behavior against a goal and correct *during* the action, not only after — feedback as steering, not as post-mortem. **Margaret Hamilton**'s Apollo guidance software is the engineering archetype: her priority-display and error-detection design let the flight computer recognize mid-descent that it was overloaded and shed the wrong tasks, surfacing the situation so a human could decide whether to continue or abort *before* touchdown — a checkpoint before an irreversible action, executed in real time on the way to the Moon. **Karl Johan Åström**'s adaptive control closes the loop the chapter recommends in §7.4: a controller that watches a process drift and adjusts its own parameters step by step to keep the trajectory on target. Trajectory evaluation is cybernetics with a tool-calling agent in the loop.

Two cautions keep the rule honest. First, *detecting* irreversibility soundly is itself unsolved — knowing which actions are truly unrecoverable, so the gate fires reliably without missing one. An allow-list of irreversible action *names* is a start, but a generic `run_command` tool can hide a `rm -rf` inside an argument the name does not reveal. Second, over-gating is its own failure. If you checkpoint everything, the human approving turns every action into a rubber stamp, and you have built the approval-fatigue problem this book treats in Ch. 10 — a reviewer who clicks "approve" reflexively is not a validator. The skill is gating the *irreversible* set tightly and leaving the reversible majority to flow, so each checkpoint still commands attention.

> **Misconception to dislodge.** "I'll review the agent's actions afterward." For reversible actions, fine. For irreversible ones, *afterward* is too late by definition — the side effect has already happened. The checkpoint must precede the action, at the last point a denial still prevents it. This is not a heuristic; it is the only hard guarantee in agentic validation.

---

## 7.4 Automatic trajectory judges: how well they work, and where they fail

For the failures no deterministic rule catches — was the *plan* sound, was the tool choice right given the goal, did a well-formed wrong value propagate — you reach for an automatic judge of the trajectory. Three forms exist, in increasing process-awareness:

- **LLM-as-judge over the transcript.** Feed the action/observation log to a model and ask whether the task was completed correctly. Outcome-leaning; cheap.
- **VLM-as-judge over screenshots.** For web/computer-use agents, judge the visual trajectory (the sequence of screens), not just text. WebArena and Android-in-the-Wild are the reference settings.
- **Agent-as-a-Judge** (Zhuge et al. 2024, arXiv:2410.10934). Extends LLM-as-a-judge with agentic features that supply **intermediate feedback across the whole task-solving process** rather than scoring only the outcome. On their DevAI benchmark (55 automated AI-development tasks, 365 hierarchical requirements) it dramatically outperforms plain LLM-as-a-judge on code-generation agents — the direct mechanism for *step-level*, not endpoint, evaluation. Pan et al. (2024, arXiv:2404.06474) close the loop further, feeding a model-based evaluator's judgment back to refine the agent — Åström's adaptive controller, instantiated.

How much can you trust them? AgentRewardBench (Lù et al. 2025, arXiv:2504.08942) is the calibration study — it benchmarks these automatic judges *against expert human annotations* on web-agent trajectories, and its figures are the ones to cite (not the loose "74–93%" that floats around):

- **Inter-annotator agreement: 89.3%** on trajectory success (the human ceiling — judges cannot be expected to beat this).
- **Best LLM judge: ~80.6%** accuracy versus the WebArena rule-based evaluator.
- **VLM-based methods: up to 82.1% on WebArena, 92.9% on Android-in-the-Wild.**
- **Agreement degrades as trajectory length grows** — screenshot/token overload, or over-focus on the final frame, makes long runs the hardest to judge.

That last finding is the load-bearing one. Judges are most reliable on short trajectories and least reliable on long ones — and long trajectories are precisely where agents are most useful and most dangerous (AgentBench's long-horizon bottleneck). So the judge's reliability curve runs *opposite* to where you need it most. Treat trajectory judges the way Ch. 6 treats retry loops and the way Ch. 8 will treat LLM-as-judge generally: a useful screening instrument that raises your odds of catching a bad path, never a certification.

And there is a circularity to keep in view, foreshadowing Ch. 8: when you use an *agent* to judge an *agent*, the two may share failure modes — the same blind spot in planning that produced the bad trajectory may be the blind spot that fails to flag it. A judge built from the same model family as the actor is not an independent check. Chapter 8 names this problem in full; here, note only that Agent-as-a-Judge buys process-awareness at the risk of shared blindness.

> **Misconception to dislodge.** "The trajectory judge passed it, so the run was good." The best judges agree with humans ~80–90% on *short* trajectories and worse on long ones. A judge pass shifts your probability of a good run upward; it does not certify one. Reserve the deterministic gates (loop detection, allow-list, irreversibility checkpoint) for the guarantees, and use the judge for the rest — knowing the rest is statistical.

---

## 7.5 The honest limit: benchmark success is not production reliability

Suppose your agent scores well — WebArena task completion looks strong, the trajectory judge mostly approves. You are not done, and the gap is the chapter's sharpest warning. A single benchmark run is one trajectory; production is the same agent run thousands of times against a shifting world, and the two numbers can be wildly different.

The CLEAR-framework study (arXiv:2511.14136 — Cost, Latency, Efficacy, Assurance, Reliability) reframes agent evaluation around exactly this. Its headline reliability finding: agent performance drops from **60% on a single run to 25% under 8-run consistency** — i.e., asked to succeed *eight times in a row*, the same agent that "passes" once succeeds only a quarter as reliably. The study also reports ~50× cost variation for similar precision, and that accuracy-only optimization yields agents 4.4–10.8× more expensive; it claims its multi-dimensional score predicts production success at ρ=0.83 versus ρ=0.41 for accuracy-only (expert N=15, 300 enterprise tasks, 6 agents). `[verify — this is a fresh Nov-2025 preprint; the 60%→25% reliability collapse is its actual headline, not the "37% lab-to-production gap" that has been misattributed to it. Treat all figures as early-stage.]`

The mechanism is intuitive once stated. A trajectory is a product of per-step success probabilities, and small per-step unreliability compounds over a horizon: even a 95%-reliable step, taken twenty times, completes the chain only ~36% of the time. Single-run benchmark numbers report the *best* the agent can do; production reliability reports what it does *repeatedly*, and the second is the number a side-effecting system must be held to. Reporting the single-run figure as the agent's reliability is the agentic analogue of the single-prompt-optimism failure from the companion brittleness chapter — a point estimate masquerading as a capability claim.

> **Misconception to dislodge.** "It passed the benchmark, so it's production-ready." A benchmark pass is one lucky trajectory; production is a distribution of trajectories under repetition and drift. The 60%→25% collapse is the gap between "succeeded once" and "succeeds reliably." Evaluate repeated-run consistency, cost variance, and the trajectory — not a single green run — before you trust an agent with irreversible actions.

---

## Exercises

1. **(Understand / Evaluate)** You are handed this report: *"Our deployment agent was evaluated on 100 tasks; 94 reached the correct final state, so it is 94% reliable."* (a) State exactly what "correct final state" does and does not certify for an agent. (b) Name two trajectory failures from §7.2 that this metric is structurally blind to, and for each say which check (deterministic or judge) would catch it. (c) Rewrite the reliability claim in terms a side-effecting production system should actually be held to, citing the 60%→25% finding.

2. **(Evaluate)** Here is a trajectory log (you may sketch one): `query` → `update` → lock_error → `query` → `update` → lock_error → `drop_table` → `create_table` → `insert` → final_state_correct. (a) Label each named failure mode present (loop, scope violation, wrong-tool, error propagation, irreversibility). (b) For each, state whether a deterministic gate or a trajectory judge catches it, and write the rule or the judge prompt. (c) Identify the single point where a checkpoint would have prevented an unrecoverable loss, and explain why validating *after* that step is meaningless.

3. **(Create, produce-something)** Take a concrete agent you can specify (a file-management, ops, or coding agent) with a tool list of at least six tools. Produce: (i) an explicit allow-list and a written, defensible definition of its *scope* — including at least one boundary case where "helpful extra" shades into "scope violation," and how you resolved it; (ii) the `IRREVERSIBLE` set for its tools, including at least one tool (like a generic shell command) whose irreversibility is hidden in its *arguments* rather than its name, and how your checkpoint detects that; (iii) a one-paragraph justification of why your checkpoint set is tight enough to avoid approval fatigue (Ch. 10) yet complete enough to gate every unrecoverable action.

4. **(Evaluate)** You must choose a trajectory judge for a long-horizon web agent (typical run: 30+ steps). Using the AgentRewardBench figures, (a) state the agreement number you should *expect* and why it is lower than the 89.3% inter-annotator ceiling and the ~80–93% short-run figures; (b) decide whether to use an LLM judge, a VLM judge, or Agent-as-a-Judge, and defend the choice on both reliability and the circularity risk; (c) name one deterministic check you would run *regardless* of the judge, because it offers a guarantee the judge cannot.

---

## What would change my mind

A demonstration that, for current frontier agents on realistic long-horizon tasks, automatic trajectory judges hold their accuracy *as trajectories lengthen* — that the AgentRewardBench length-degradation has been engineered away, so a judge is roughly as reliable on a 50-step run as on a 5-step one. If that held, the "judge is statistical, reserve deterministic gates for guarantees" framing would soften: trajectory judges would approach certification on exactly the long runs where they currently fail, and the irreversibility checkpoint would become the *only* irreducibly-manual gate rather than one of several. I would also revise the 60%→25% pessimism if a replicated, multi-team study showed single-run benchmark success transferring to repeated-run production reliability for a class of agents — but the compounding-over-horizon mechanism makes me expect some collapse to persist, and the CLEAR figure is a fresh single preprint, not settled. As of this writing, both the length-degradation and the reliability collapse are real. `[verify both against the reader's-date literature.]`

## Still puzzling

- **Defining scope crisply.** The boundary between acceptable autonomy and a scope violation resists deterministic specification; we enforce allow-lists well but draw their edges by hand and by taste.
- **Sound irreversibility detection.** Knowing *which* actions are truly unrecoverable — especially when irreversibility hides in a generic tool's arguments — so the checkpoint fires reliably without over-gating, is unsolved.
- **Trajectory-judge reliability on long horizons.** Agreement degrades exactly where agents are most useful and most dangerous; we lack a judge that is trustworthy at the lengths that matter most (AgentRewardBench).
- **The lab-to-production gap.** Why single-run benchmark success collapses under repetition (60%→25%), and what evaluation actually predicts production reliability, is early-stage (CLEAR claims ρ=0.83; one preprint).
- **Agent-as-judge circularity.** Whether using an agent to judge an agent introduces shared failure modes that systematically hide the worst trajectories — the Ch. 8 problem, unsolved for agents specifically.

---

## References

- Liu, X., Yu, H., et al. (2023). [AgentBench: Evaluating LLMs as Agents](https://arxiv.org/abs/2308.03688). arXiv:2308.03688. *ICLR 2024.* (Eight interactive environments; long-horizon reasoning is the bottleneck — failure is a trajectory phenomenon.)
- Zhou, S., Xu, F. F., et al. (2023). [WebArena: A Realistic Web Environment for Building Autonomous Agents](https://arxiv.org/abs/2307.13854). arXiv:2307.13854. *ICLR 2024.* (Reproducible web environments; functional task-completion scoring.)
- Zhuge, M., Zhao, C., et al. (2024). [Agent-as-a-Judge: Evaluate Agents with Agents](https://arxiv.org/abs/2410.10934). arXiv:2410.10934. (Intermediate feedback across the whole task-solving process; DevAI benchmark.)
- Lù, X. H., Kazemnejad, A., et al. (2025). [AgentRewardBench: Evaluating Automatic Evaluations of Web Agent Trajectories](https://arxiv.org/abs/2504.08942). arXiv:2504.08942. (Calibrated judge figures: 89.3% inter-annotator; ~80.6% best LLM judge; 82.1%/92.9% VLM; degrades with length.)
- Pan, J., Zhang, Y., et al. (2024). [Autonomous Evaluation and Refinement of Digital Agents](https://arxiv.org/abs/2404.06474). arXiv:2404.06474. (Model-based trajectory evaluators feeding back to refine the agent.)
- *Beyond Accuracy: A Multi-Dimensional Framework for Evaluating Enterprise Agentic AI Systems* (CLEAR) (2025). [arXiv:2511.14136](https://arxiv.org/abs/2511.14136). (60%→25% single-run vs. 8-run reliability collapse; cost variance; ρ=0.83 vs 0.41. `[verify — fresh Nov-2025 preprint; the "37% lab-to-production gap" is a misattribution — use 60%→25%.]`)
- Wiener, N. (1948). *Cybernetics: Or Control and Communication in the Animal and the Machine.* MIT Press. (Feedback as mid-action steering — the intellectual root of trajectory evaluation and the checkpoint.)

---

**Tags:** agentic-validation, trajectory-evaluation, agentbench, webarena, agent-as-a-judge, agentrewardbench, loop-detection, scope-violation, wrong-tool, error-propagation, irreversibility-checkpoint, validate-before, lab-to-production-gap, reliability-collapse, judge-degrades-with-length
