# Chapter 5 — Validating Reasoning Chains

*Execute if you can, vote if you can't, and never trust the model to grade its own work*

Here is a pattern I want you to recognize, because it is the most common validation mistake in the field, and it is dressed up to look like rigor.

A team builds a quantitative-reasoning assistant and adds a "reflection" feature. The model solves a word problem, then re-reads its own solution under the instruction: *"Review your reasoning above. Identify any errors and correct them."* In demos it looks like magic: the model catches a dropped negative sign, rewrites the step, and arrives at the right answer. They call it a validation layer — "the model checks its work before answering."

When they finally run a controlled evaluation — same problems, with and without the reflection step, scored against known answers — the result is not what the demos promised. On problems the model got right the first time, reflection sometimes talks it *out* of the correct answer: it finds an "error" that was not an error, rewrites a sound step, and lands on a wrong total. Net accuracy does not improve. On the hardest problems it goes *down.*

The reflection step was not validating anything. It was resampling the model's own judgment and calling the second sample a check. The team had made no error of reasoning. They had made an error of **validation** — and it is the error this chapter exists to prevent.

To correct a mistake, you need a signal that points away from it. The model's only available signal, when asked to review its own reasoning with nothing external, is the same plausibility judgment that produced the error in the first place. A biased estimator sampled twice is still biased. The structure of the problem makes ungrounded self-correction unreliable not as an empirical accident but as a matter of mechanism.

What works instead — and when — is what the rest of this chapter is about.

---

## Why ungrounded self-correction fails

Huang et al. (2024), "Large Language Models Cannot Self-Correct Reasoning Yet" (ICLR 2024), studied *intrinsic* self-correction: the model revises its own answer using only its own capabilities, with no external feedback, no tool, no ground-truth signal. The finding is blunt. On reasoning tasks, intrinsic self-correction does not reliably improve performance, and it sometimes degrades it. Prompting a model to "review and fix" its reasoning is not a free validation layer; on average it is a coin flip that occasionally costs you a correct answer.

The mechanism generalizes. A correction step needs a *gradient* — some signal indicating which direction is "more correct." When you run unit tests, the gradient is the test result: pass or fail, determined externally and deterministically. When you run a compiler, the gradient is the type error. When a model critiques its own reasoning with nothing external, the only gradient available is its internal estimate of plausibility. But plausibility is exactly what generated the original answer. If the first pass produced a fluent-but-wrong chain, the model already found that chain plausible — so re-asking it to judge plausibility cannot reliably move away from the error.

This is not a claim that self-correction is useless in all situations. It is a claim about *ungrounded* self-correction on reasoning. When correction is grounded in external feedback — the model fixes its code after seeing the failing test, or revises its proof after the checker rejects it — a different mechanism operates, and it works, because the gradient is real. Part of what Huang et al. demonstrate is that earlier literature conflated the two, attributing to "the model correcting itself" gains that actually came from the tool or the test.

Hold the distinction firmly: the question is never "did the model reconsider?" It is "what external signal did the reconsideration condition on?" No external signal, no validation.

![Two parallel panels. Left: a model's output loops directly back to its own input, a closed self-loop with no external signal. Right: an identical model routes its output through an external oracle that returns real feedback. The only difference is whether the correction loop passes through an external node.](../images/05-validating-reasoning-chains-fig-02.png)
![Ungrounded self-correction loops back on its own plausibility; grounded correction routes through an external oracle that supplies a real gradient.](images/05-validating-reasoning-chains-fig-02.png)
*Figure 5.2 — Ungrounded self-correction loops back on its own plausibility; grounded correction routes through an external oracle that supplies a real gradient.*

<!-- → [FIGURE: Two correction loops side by side — left: ungrounded (model → plausibility check → same model, no gradient); right: grounded (model → external oracle → error signal → model revision); caption: "The only difference is the arrow from outside — that arrow is what makes correction work"] -->

---

## When a deterministic oracle exists

When a deterministic oracle exists, reasoning validation is not merely good — it is *sound*. Two flavors, strongest first.

**Formal proof: send it to a checker.** In a proof assistant — Lean, Coq, Isabelle — a proof is a formal object and the checker either accepts it or rejects it. There is no "looks plausible." There is type-checks or does-not-type-check, a sound binary verdict. This archetype of mechanical validation is old. In 1967 the Dutch mathematician Nicolaas Govert de Bruijn built **Automath**, widely considered the first proof checker — a machine that read a proof and accepted each step only if it followed by the rules, nothing taken on trust. By the mid-1970s an entire analysis textbook had been formally verified in it. The theoretical foundation traces to Gerhard Gentzen's natural deduction and cut-elimination (1934–35): inference reduces to local, mechanically-verifiable rules. The engineering descendant is Leslie Lamport's TLA+, bringing mechanical proof and model-checking to working systems.

The catch is the **auto-formalization gap**. The oracle is perfect, but getting your reasoning *into* the oracle's language — translating a natural-language argument into Lean, or a real computation into a faithful program — is itself unreliable and is currently the bottleneck on extending deterministic validation to reasoning in the wild. The oracle does not lie; the translator does.

**Execution: make the interpreter the oracle (PAL).** Gao et al. (2023), "PAL: Program-aided Language Models" (ICML 2023), diagnosed a specific and reproducible failure: large models *decompose* problems well — they correctly figure out what to compute — but they make arithmetic and logic mistakes while *executing* the computation in natural-language chain-of-thought. The remedy follows directly from the diagnosis: let the model do what it is good at (decomposition) and offload what it is bad at (execution) to a runtime that cannot make arithmetic errors.

The model emits a *program* as its reasoning chain; a Python interpreter runs it; the interpreter's output is the answer. Consider a word problem:

*"A bakery sold 3 dozen muffins in the morning and 17 in the afternoon, then received a delivery of 50 more. If they started with 12, how many do they have now?"*

A natural-language chain might say "3 dozen is 36, plus 17 is 53, started with 12 so 65 sold, delivery 50…" — and somewhere in that prose the model can transpose a digit or lose track of a sign. The PAL version emits:

```python
morning = 3 * 12       # 3 dozen
afternoon = 17
sold = morning + afternoon
start = 12
delivery = 50
remaining = start - sold + delivery
print(remaining)
```

Whatever the model's mental arithmetic would have been, it is now irrelevant: `remaining` is computed by the interpreter, deterministically, every time. You have not eliminated error; you have *moved* it to a layer where you can see it and the runtime catches the rest. The validation question shifts from "did the model add correctly?" (which you cannot check without redoing the arithmetic) to "did the model translate the problem into the right program?" — and the program is short, inspectable, and re-runnable.

> **The residue to watch.** A program can run cleanly while solving the wrong problem. A Lean proof can verify a theorem that is not the one you meant to state. The deterministic oracle certifies *the formalized claim*, not *your intent*. Inspect the translation, not just the verdict.

<!-- → [FIGURE: PAL architecture — word problem → model emits Python program → interpreter executes → numeric answer; contrast with natural-language chain where arithmetic error can occur at any step; caption: "Decomposition stays with the model; execution moves to the interpreter, which cannot make arithmetic errors"] -->

---

## When no oracle exists: self-consistency

Most reasoning you will validate is not a formal proof and cannot be reduced to a short program. But a large and useful subset has a weaker property: the *final answer* is comparable across runs — it is a number, a label, a discrete choice. For that subset there is a judgment-free validator that requires no oracle at all.

![One prompt fans out into several independent reasoning paths. Most answer tokens converge onto a single majority node, tallied by a count; one or two idiosyncratic outliers scatter to separate nodes. Agreement correlates with correctness without any model self-assessment.](../images/05-validating-reasoning-chains-fig-03.png)
![Self-consistency: sample many independent paths, let correct chains converge on a majority vote while idiosyncratic errors scatter.](images/05-validating-reasoning-chains-fig-03.png)
*Figure 5.3 — Self-consistency: sample many independent paths, let correct chains converge on a majority vote while idiosyncratic errors scatter.*

Wang et al. (2022), "Self-Consistency Improves Chain of Thought Reasoning in Language Models" (ICLR 2023), replace greedy single-path decoding with a simple aggregation: sample a *diverse set* of reasoning paths from the model, then take the **majority-vote answer** across them. The intuition is that a hard problem has many valid routes to the correct answer, but errors tend to be idiosyncratic — different wrong chains go wrong in different directions, while correct chains converge on the same destination. Agreement across independently-sampled trajectories correlates with correctness, and crucially requires no model self-assessment. Nobody grades anything; you count.

The reported gains over plain chain-of-thought were large: roughly 17.9 points on GSM8K, 11.0 on SVAMP, 12.2 on AQuA. Treat these as historical, model-specific figures — the mechanism has aged well even as the numbers have aged.

In practice the procedure is short:

```python
from collections import Counter

def self_consistent_answer(model, prompt, n=20, temperature=0.7):
    samples = [model.generate(prompt, temperature=temperature) for _ in range(n)]
    answers = [extract_final_answer(s) for s in samples]
    votes = Counter(answers)
    answer, count = votes.most_common(1)[0]
    return answer, count / n   # vote share is a cheap confidence signal
```

The vote share (`count / n`) is a usable crude confidence reading: a 19-of-20 consensus is a very different situation from a 6-5-4-3-2 split, and you can route low-consensus cases to a human. The samples must be *independent* (raised temperature, no shared scratchpad), and the answer must be *comparable* (you can tell when two samples agree). Where those hold, you get robustness for the price of N forward passes and no judgment.

<!-- → [CHART: Two vote distributions contrasted — left: idiosyncratic noise (correct answer ~15/20 votes, wrong answers scattered 1–2 each); right: systematic bias (wrong answer ~17/20 votes, correct scattered); caption: "Self-consistency works on the left; it confidently launders the error on the right — more votes cannot fix a shared prior"] -->

But self-consistency has a real failure boundary, and you need to know exactly where it is. Voting assumes the model's errors are *noise* — random, idiosyncratic, canceling. When the model is *systematically* biased — when most independent samples make the same wrong move because the error is baked into the model's prior — voting launders the popular wrong answer into a confident wrong answer. Self-consistency raises confidence in the majority; it does not check whether the majority is right. More votes make you more sure and no more correct, when the bias is shared. This is precisely the cell where you want a real oracle and do not have one.

---

## The decision: verify by domain

Put the mechanisms in order. The choice of validator is not a matter of taste; it is dictated by one question asked repeatedly — *is ground truth mechanically available, and at what granularity?*

![Three cascading decision diamonds choose a reasoning validator: deterministic oracle yes leads to proof checker or executor; otherwise comparable answer yes leads to self-consistency; otherwise labelable steps yes leads to a process reward model, no leads to human review. Model self-critique alone is drawn isolated and struck through as forbidden.](../images/05-validating-reasoning-chains-fig-01.png)
![The verify-by-domain decision tree: oracle, comparable answer, or labelable steps pick the validator — self-critique alone is forbidden.](images/05-validating-reasoning-chains-fig-01.png)
*Figure 5.1 — The verify-by-domain decision tree: oracle, comparable answer, or labelable steps pick the validator — self-critique alone is forbidden.*

**1. Can the conclusion be checked by a deterministic oracle?**
- Yes, and it is a formal proof → **proof/type checker** (Lean/Coq/Isabelle). Sound yes/no. Strongest validation that exists; bottleneck is faithful formalization.
- Yes, and it is computation → **execute it** (PAL). The interpreter is the oracle; inspect the translation, not the arithmetic.

**2. No oracle — is the final answer comparable across independent samples?**
- Yes → **self-consistency** (sample N, majority vote). Judgment-free robustness; watch for systematic bias.

**3. No comparable final answer — are intermediate steps labelable as correct/incorrect?**
- Yes → **Process Reward Model** for step-level scoring (Chapter 9). Lightman et al. (2023), "Let's Verify Step by Step," showed process supervision — feedback on each step — beats outcome supervision on hard math. Previewed here; full treatment in Chapter 9.
- No → **human review.** This is the cell where validation is weakest. Mark it explicitly.

One branch is forbidden at every leaf: **model self-critique alone** — asking the model to grade its own reasoning with no external signal (Huang et al.). Every legitimate leaf grounds in something external: an interpreter, a checker, the agreement of independent samples, a step label, a human. Ungrounded introspection is the only path the evidence rules out. Draw it and cross it out.

<!-- → [FIGURE: Decision tree for reasoning validation — root: "Is a deterministic oracle available?"; left branch: proof → proof checker, computation → execute (PAL); right branch: "Is the final answer comparable?" yes → self-consistency, no → "Are steps labelable?" yes → PRM (Ch. 9), no → human review; crossed-out branch from root labeled "model self-critique alone"] -->

Step 3's question — "are intermediate steps labelable?" — is itself a ground-truth-availability test. PRMs presuppose that a human or model can mark an intermediate step as correct or incorrect. In math and code that label is usually well-defined: a step is correct if it preserves the truth of the derivation. In open-ended legal, strategic, or commonsense reasoning it often is not. There may be no fact of the matter about whether an intermediate inference is "correct," only whether the whole argument is persuasive. Where the step label is undefined, PRMs have nothing to score, and you are in the weakest cell — no oracle, no comparable answer, no labelable step. That cell is not a gap in technique; it is a gap in *ground truth*, and pretending a technique fills it is the failure the book exists to prevent.

---

## The same problem, three ways

To make the escalation concrete: one arithmetic word problem, validated three ways, weakest to strongest.

![Three stacked rungs read bottom to top as increasing validation strength: a single greedy chain with no validation and a fragile dashed edge; self-consistency giving statistical robustness with a solid edge; PAL execution as a sound deterministic oracle with a heavy edge. An upward arrow spans the stack.](../images/05-validating-reasoning-chains-fig-04.png)
![The validation ladder: single greedy chain (none), self-consistency (statistical), PAL execution (sound oracle), climbing as ground truth allows.](images/05-validating-reasoning-chains-fig-04.png)
*Figure 5.4 — The validation ladder: single greedy chain (none), self-consistency (statistical), PAL execution (sound oracle), climbing as ground truth allows.*

**Single greedy chain (no validation).** Ask once, temperature 0, take the natural-language answer. Fragile: one dropped sign and the answer is wrong with no signal that anything failed.

**Self-consistency (statistical, judgment-free).** Sample 20 chains at temperature 0.7, majority-vote the final number. Robust to idiosyncratic arithmetic slips — they scatter; the right answer concentrates. Still blind to systematic bias, and gives you a confidence reading (vote share) for free. No oracle required.

**PAL execution (deterministic oracle).** Have the model emit a Python program and run it. The arithmetic error class is *gone* — eliminated, not averaged down — because the interpreter computes the value. Your remaining risk is mistranslation, which is inspectable in a way that mental arithmetic is not.

The ladder is the lesson: from no validation, to statistical agreement, to a sound oracle. You climb as high as ground truth lets you. For an arithmetic word problem you can reach the top rung. For "what is the strongest legal argument against this clause?" there is no top rung — no interpreter, no proof checker, and arguably no comparable final answer to vote on — and the honest move is to say so and route to a human, not to dress up self-critique as a check.

The book's thesis lands here with unusual sharpness: **validation works where ground truth is mechanically available and fails where it isn't.** Reasoning is the output type that makes the boundary visible, because for the same class of problem you can sometimes have a perfect oracle and sometimes have nothing at all. That boundary is not an implementation detail. It is the governing fact of the chapter, and knowing where it falls is the skill.

---

## LLM Exercises

**Exercise 5.1 (Understand/Analyze).** A colleague's pipeline does: generate solution → prompt the model *"Find and fix any errors above"* → return the revised solution, and labels the second step "automated verification." (a) Name the precise mechanism by which this can *reduce* accuracy, citing Huang et al. (2024). (b) State what would have to be added to the second step to turn it from resampling into genuine validation. (c) Give one task type where the colleague's setup would actually help and explain why the gradient is real there.

**Exercise 5.2 (Apply).** Take ten arithmetic word problems with known answers. Implement two validators: (i) self-consistency with N=20 majority vote, and (ii) PAL — have the model emit a Python program and execute it. Produce a small table: per-problem, the single-greedy answer, the self-consistency answer with vote share, and the PAL answer. Write one paragraph on which problems the three methods disagreed on and why — and identify any problem where self-consistency confidently agreed on a *wrong* answer (systematic bias in action).

**Exercise 5.3 (Evaluate).** For each of the following reasoning outputs, walk the decision tree and name the validator you would use and what it cannot catch: (a) "Prove that the sum of the first n odd numbers is n²"; (b) "Compute the total interest on this amortization schedule"; (c) "Classify which of these 200 support tickets are billing-related"; (d) "Argue whether this acquisition is strategically wise." For (d), state explicitly what cannot be certified and who owns the residual risk.

**Exercise 5.4 (Evaluate).** Self-consistency "launders systematic bias." Construct a concrete example: a problem where you would expect most independent samples to converge on the same wrong answer (a question with a tempting but incorrect intuitive answer works well). Predict the vote distribution, then run it if you can. Explain why adding more samples would not help, and name the kind of validator that *would* catch this class of error.

---

## References

- Huang, J., Chen, X., Mishra, S., Zheng, H. S., Yu, A. W., Song, X., & Zhou, D. (2024). *Large Language Models Cannot Self-Correct Reasoning Yet.* ICLR 2024. arXiv:2310.01798. — ungrounded intrinsic self-correction does not reliably improve and can degrade.
- Gao, L., Madaan, A., Zhou, S., Alon, U., Liu, P., Yang, Y., Callan, J., & Neubig, G. (2023). *PAL: Program-aided Language Models.* ICML 2023. arXiv:2211.10435. — model decomposes; a Python interpreter executes; the interpreter is the oracle.
- Wang, X., Wei, J., Schuurmans, D., Le, Q., Chi, E., Narang, S., Chowdhery, A., & Zhou, D. (2022). *Self-Consistency Improves Chain of Thought Reasoning in Language Models.* ICLR 2023. arXiv:2203.11171. — majority vote over diverse sampled chains; +17.9 GSM8K, +11.0 SVAMP, +12.2 AQuA (historical figures).
- Lightman, H., et al. (2023). *Let's Verify Step by Step.* arXiv:2305.20050. — process supervision beats outcome supervision on hard math (full treatment in Chapter 9).
- de Bruijn, N. G. (1968). *AUTOMATH* — the first proof checker; Landau's analysis textbook formally verified in it by 1977 (van Benthem Jutting).
- Gentzen, G. (1934–35). *Untersuchungen über das logische Schließen* — natural deduction and cut-elimination; Lamport, L. — TLA+ (mechanical proof and model-checking for systems).

