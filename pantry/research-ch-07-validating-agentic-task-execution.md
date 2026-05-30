# Research: Chapter 07 — Validating Agentic Task Execution
## Validating Output from AI Systems
**Chapter one-line:** Evaluate the *trajectory* (sequence of actions), not just the final output; checkpoint before irreversible actions; catch loops, scope violations, wrong-tool, and error propagation.
**Research date:** 2026-05-29

---

## 1. Primary Sources

### Foundational papers and texts

- **Liu, Yu, et al. (2023). "AgentBench: Evaluating LLMs as Agents."** arXiv:2308.03688 (submitted Aug 2023; ICLR 2024). VERIFIED. A multi-dimensional benchmark of 8 distinct interactive environments assessing LLM-as-agent reasoning and decision-making over multi-step tasks. Headline finding: **poor long-term reasoning, decision-making, and instruction-following are the main obstacles** to usable agents — i.e., failures accumulate over the *trajectory*, not at a single output. The empirical basis for evaluating the sequence of actions rather than the endpoint. https://arxiv.org/abs/2308.03688

- **Zhou, Xu, et al. (2023). "WebArena: A Realistic Web Environment for Building Autonomous Agents."** arXiv:2307.13854 (submitted Jul 2023; ICLR 2024). VERIFIED. Reproducible, fully-functional websites across four domains (e-commerce, forums, software dev, CMS); benchmark tasks evaluate **functional correctness of task completion**, not surface output. The reference environment in which trajectory-level evaluation (below) is measured. https://arxiv.org/abs/2307.13854

- **Lù, Kazemnejad, et al. (2025). "AgentRewardBench: Evaluating Automatic Evaluations of Web Agent Trajectories."** arXiv:2504.08942. VERIFIED (corrects the loose "VLM-judge 74–93%" claim). Benchmarks automatic (LLM/VLM) judges *of trajectories* against expert human annotations on multiple dimensions. Key calibrated numbers: inter-annotator agreement **89.3%** on success (GPT-4o trajectory on WebArena); the best LLM judge reached **80.6%** accuracy vs the WebArena rule-based evaluator; VLM-based methods reach up to **82.1% on WebArena** and **92.9% on Android-in-the-Wild**; agreement **degrades as trajectory length grows** (screenshot/token overload or over-focus on the final frame). Use these specific figures, not the unsourced "74–93%." https://arxiv.org/abs/2504.08942

- **Zhuge, Zhao, et al. (2024). "Agent-as-a-Judge: Evaluate Agents with Agents."** arXiv:2410.10934 (Oct 2024). VERIFIED — the "agent-as-judge" source the prompt asked us to find. Extends LLM-as-a-Judge with agentic features that provide **intermediate feedback across the whole task-solving process**, not just the final outcome; introduces the DevAI benchmark (55 automated AI-dev tasks, 365 hierarchical requirements) and reports it dramatically outperforming LLM-as-a-Judge on code-generation agents. The direct mechanism for trajectory-level (vs outcome-level) automatic evaluation. https://arxiv.org/abs/2410.10934

- **Pan, Zhang, et al. (2024). "Autonomous Evaluation and Refinement of Digital Agents."** arXiv:2404.06474. VERIFIED. Builds model-based evaluators that judge digital-agent trajectories and feed the judgment back to refine the agent — the loop the chapter recommends, with the caveat that the evaluator is itself fallible. https://arxiv.org/abs/2404.06474

### Key empirical cases

- **"Beyond Accuracy: A Multi-Dimensional Framework for Evaluating Enterprise Agentic AI Systems."** arXiv:2511.14136. VERIFIED that the ID resolves to a real paper (CLEAR framework: Cost, Latency, Efficacy, Assurance, Reliability) — **but the TIKTOC's "37% lab-to-production gap for multi-agent" is NOT this paper's headline.** What the paper actually reports: agent performance drops from **60% (single run) to 25% (8-run consistency)**; ~50× cost variation for similar precision; accuracy-only optimization yields agents 4.4–10.8×more expensive; CLEAR predicts production success at ρ=0.83 vs ρ=0.41 for accuracy-only (expert N=15, 300 enterprise tasks, 6 agents). **Correct the "37%" claim** to the verified 60%→25% reliability collapse, or drop the specific number. Flagged below. ID is **2511** = Nov 2025; relative to the 2026-05-29 research date this is recent-past, not future-dated — but it is a fresh preprint, treat as early-stage. https://arxiv.org/abs/2511.14136
- **AgentBench's "long-term reasoning is the bottleneck"** — the canonical empirical statement that agent failure is a trajectory phenomenon.

---

## 2. The Core Concept — State of the Field

### What is settled
- **Final-output-only evaluation is insufficient for agents.** A correct end state can be reached through a broken, unsafe, or wasteful trajectory; a wrong end state hides where the agent went off the rails (AgentBench; Agent-as-a-Judge).
- **Long-horizon execution is where agents fail** — error propagation, loops, and instruction drift accumulate across steps (AgentBench).
- **Irreversible actions need a checkpoint.** Validating *before* a destructive/external action is categorically different from validating output after the fact; once executed, there is nothing left to reject.
- **Automatic trajectory judges work moderately well and degrade with length** (AgentRewardBench: ~80–93% depending on benchmark and modality, falling on long trajectories).

### What is disputed
- **How reliable trajectory judges are in production.** ~80–93% agreement on benchmarks; the lab-to-production gap (2511.14136) suggests single-run benchmark numbers overstate real reliability (60%→25% across repeated runs).
- **Whether agent-judges introduce circularity** — using an agent to judge an agent risks shared failure modes (foreshadows Ch. 8).
- **What counts as a "scope violation" vs legitimate autonomy** — the boundary between an agent helpfully doing extra and an agent exceeding its mandate is not crisply defined.
- **The "37%" lab-to-prod figure itself** — not supported by the cited paper; disputed/uncorroborated as stated.

### What has changed recently (last 5 years)
- 2023: AgentBench and WebArena establish reproducible, functional-correctness agent benchmarks — trajectory evaluation becomes measurable.
- 2024: Agent-as-a-Judge and autonomous trajectory evaluators (Pan et al.) move from outcome scoring to step-level/process scoring of agent runs.
- 2025: AgentRewardBench calibrates how well those automatic judges actually agree with humans (and where they fail — long trajectories).
- Late 2025: enterprise-evaluation frameworks (CLEAR, 2511.14136) reframe the problem around *reliability under repetition, cost, and irreversibility*, exposing the lab-to-production reliability collapse.

---

## 3. Application Domain Examples
- **Web/computer-use agents (WebArena, Android-in-the-Wild):** judge the action sequence (clicks, form fills, navigation), not just the final screen; VLM judges reach ~82–93% agreement, falling on long runs.
- **Coding agents:** Agent-as-a-Judge over the development trajectory (which files touched, which tests run) vs only checking the final diff compiles. (Overlap: the coding-agents companion book's review/CI chapters.)
- **Ops/automation agents:** checkpoint before any destructive or external side-effecting action (delete, deploy, send, pay) — human or deterministic gate before irreversibility.
- **Multi-agent enterprise pipelines:** measure reliability across repeated runs (60%→25% collapse), not a single lucky trajectory.

---

## 4. The Book's Thesis Connection
Agentic execution is where the thesis bites hardest: **evaluate the trajectory, not just the final output**, because for agents the "output" is a whole sequence of actions with side effects, and ground truth on the *final state* says nothing about whether the path was safe, in-scope, or reproducible. Ground truth is only partially mechanically available — you can deterministically detect some failures (loops, wrong-tool calls, scope violations against an allow-list, error propagation) but correctness of the overall plan often has no oracle, so trajectory judges (LLM/VLM/agent-as-judge) are statistical and fallible (~80–93%, degrading with length). The **checkpoint-before-irreversible-action** rule is the deterministic-first principle applied to a world with side effects: you cannot retry a deletion, so you gate it before execution. The lab-to-production reliability collapse (60%→25%) is the chapter's honest limit — benchmark success does not certify production correctness.

---

## 5. The AI Wayback Machine — Candidate Figures

- **Norbert Wiener** (full name; Wikipedia: "Norbert Wiener"). Founder of cybernetics (1948) — the science of control and *feedback* in systems that act in the world. His central insight, that a system must continuously compare its actions against a goal and correct course, is exactly trajectory evaluation: judge the unfolding sequence, not just the endpoint. The intellectual ancestor of "checkpoint and feedback before the action completes." **Anchor prompt:** *"A self-steering machine constantly checking its own course against a target, correcting mid-action before it overshoots."* (Note: TIKTOC mentions Wiener was trimmed from a companion book's Ch.0 — fine to feature him here where he is directly on-thesis.)

- **Margaret Hamilton** (full name; Wikipedia: "Margaret Hamilton (software engineer)"). Led Apollo flight software; her *priority-display / error-detection* system let the guidance computer detect when it was overloaded or executing the wrong task and recover *during* the mission — famously during the Apollo 11 landing. The archetype of validating an autonomous system's *execution trajectory* in real time, with checkpoints before irreversible action. Adds gender diversity and an engineering (not theory) anchor. **Anchor prompt:** *"A flight computer mid-descent flagging that it is overloaded and shedding the wrong tasks, a human deciding whether to continue or abort before touchdown."*

- **Karl Johan Åström** (full name; Wikipedia: "Karl Johan Åström"). Pioneer of adaptive control and automatic-tuning controllers — systems that monitor their own behavior over a trajectory and adjust. Connects cybernetics to modern control engineering and to the agent-evaluator-refinement loop (Pan et al. 2024). Lesser-known; diversifies nationality (Swedish). **Anchor prompt:** *"A controller watching a process drift, adjusting its own parameters step by step to keep the trajectory on target."*

Skew note: the control/feedback lineage is mostly mid-20th-century American/European engineering; Hamilton adds gender diversity, Åström adds nationality diversity. Wiener is well-known (mitigated by his being directly on-thesis rather than a token).

---

## 6. Pedagogical Delivery Research
- **Open with a "right answer, wrong path" story:** an agent that reaches the correct final state by deleting and recreating a resource, or by looping ten times — final-output evaluation passes it; trajectory evaluation catches it. This dislodges the reader's instinct to check only the end.
- **Enumerate the failure taxonomy concretely:** loops, scope violations, wrong-tool, error propagation — each with a worked trajectory and which check (deterministic vs judge) catches it.
- **Teach the irreversibility gate as a hard rule, not a heuristic:** before any action you cannot undo, insert a checkpoint (human or deterministic). Tie to approval-fatigue (Ch. 10) so readers don't over-gate into rubber-stamping.
- **Use the calibrated judge numbers** (89.3% inter-annotator, ~80–93% judge agreement, degrading with length) to set honest expectations — automatic trajectory judges help but do not certify.
- **Confront the lab-to-prod gap** with the verified 60%→25% reliability collapse to inoculate against benchmark optimism.

---

## 7. Representation and Display Research
**Trajectory-vs-final-output evaluation diagram (specified).** Two parallel horizontal tracks for the same agent run:

- **Top track — Final-output evaluation:** a single box at the far right ("end state correct?") with a green check. Everything before it is greyed/ignored. Caption: "passes — but blind to how it got there."
- **Bottom track — Trajectory evaluation:** the full action sequence as a chain of nodes (step 1 → step 2 → … → end), with inline checks annotating specific steps:
  - a **loop** drawn as a back-arrow (step 4 → step 2), flagged red;
  - a **scope violation** node (action outside the allow-list), flagged red;
  - a **wrong-tool** node, flagged amber;
  - an **error-propagation** edge where a bad step's output feeds the next;
  - a **checkpoint gate** (a lock icon) placed *before* an irreversible action (e.g., "DELETE"), labeled "validate before — cannot retry after."
- **Judge band:** below the trajectory, a thin band labeled "automatic trajectory judge (LLM/VLM/agent-as-judge): ~80–93% agreement, degrades with length" — visually shrinking confidence as the chain lengthens.
The visual thesis: the same run passes the top track and fails the bottom; correctness lives in the path and its side effects, and the only hard guarantee is the checkpoint before irreversibility.

---

## 8. Open Questions and Research Gaps
- **Defining scope violations and acceptable autonomy** crisply enough to check deterministically.
- **Trajectory-judge reliability on long horizons** — agreement degrades exactly where agents are most useful and most dangerous (AgentRewardBench).
- **Circularity of agent-as-judge** — shared failure modes between actor and judge agents (Ch. 8 link).
- **Closing the lab-to-production gap** — why single-run benchmark success (60%) collapses under repetition (25%) and what evaluation predicts production reliability (CLEAR claims ρ=0.83, early-stage).
- **Automatic, sound detection of irreversibility** — knowing *which* actions are unrecoverable so the checkpoint fires reliably without over-gating.

---

## 9. Sourcing Notes
- arXiv:2308.03688 (AgentBench), 2307.13854 (WebArena), 2410.10934 (Agent-as-a-Judge), 2404.06474 (Pan et al.), 2504.08942 (AgentRewardBench), and 2511.14136 (CLEAR) all VERIFIED to resolve to the stated papers.
- **CORRECTION flagged:** TIKTOC's "lab-to-production gap (37% for multi-agent, arXiv:2511.14136)" is not supported by that paper. The paper's actual headline reliability finding is **60% single-run → 25% 8-run consistency**. Recommend the prose either cite 60%→25% or drop the "37%" figure. Marked [verify] historically; now resolved as **mismatch — do not assert 37%.**
- **CORRECTION flagged:** TIKTOC's "WebArena VLM-judge 74–93% agreement" is loosely stated. Calibrated figures (AgentRewardBench): inter-annotator 89.3%; best LLM judge 80.6% vs WebArena rule-based; VLM up to 82.1% (WebArena) / 92.9% (Android-in-the-Wild); degrades with trajectory length. Use these.
- arXiv:2511.14136 is a Nov-2025 preprint — relative to the 2026-05-29 research date it is recent-past, NOT future-dated; still treat as early-stage/contested per the chapter's aging-risk note.
- Wiener (cybernetics), Hamilton (Apollo priority display), and Åström (adaptive control) facts are well-established; no [verify] needed. Hamilton's correct Wikipedia disambiguation is "Margaret Hamilton (software engineer)."
