> **Voice status:** `voice-unanchored`. Root `style/` and book-level `style/` empty as of this draft.

---

# Chapter 5 — Validating Reasoning Chains

*Execute if you can, vote if you can't, and never trust the model to grade its own work*

**Author:** Nik Bear Brown
**Editor:** Nik Bear Brown

---

## Suggested titles

1. **Validating Reasoning Chains: Execute If You Can, Vote If You Can't**
2. **The Oracle You Have and the Oracle You Wish You Had: Choosing Verification by Domain**
3. **Why "The Model Checked Its Work" Means Nothing — and What to Do Instead**

---

## TL;DR

A reasoning chain is a sequence of intermediate steps that a model emits on its way to an answer, and the seductive failure is to treat that chain as its own evidence: it is long, it is articulate, each step "follows," so the conclusion must be sound. It need not be. The right validation depends entirely on whether ground truth is **mechanically available**. If the reasoning can be expressed as computation, emit a program and run it — the interpreter is a sound oracle (PAL; Gao et al. 2023). If it is a formal proof, send it to a proof checker — it type-checks or it does not (Lean/Coq/Isabelle, descendants of de Bruijn's 1967 Automath). When no oracle exists but the final answer is comparable across runs, sample many independent chains and take the **majority vote** — self-consistency, which buys robustness with zero judgment (Wang et al. 2022). What you must *not* do is ask the model to critique and revise its own reasoning with no external signal: intrinsic self-correction does not reliably improve reasoning and frequently **degrades** it (Huang et al. 2024), because the model has nothing to correct *toward* except its own sense of plausibility — the very thing that produced the error. The honest limit: when the final answer is incomparable across samples *and* intermediate steps cannot be labeled correct or incorrect (much of legal, strategic, and commonsense reasoning), you have no mechanical validator at all, and you fall back to human review. Process Reward Models (Ch. 9) are the step-level upgrade exactly where "is this step correct?" is a well-posed question — which is itself a ground-truth-availability test in disguise.

---

## Learning objectives

By the end of this chapter you should be able to:

1. **(Understand)** Explain why ungrounded self-correction fails on reasoning — the model edits toward plausibility, not correctness — and why a long, fluent chain is not evidence for its own conclusion.
2. **(Apply)** Convert a computational reasoning task into executable code so a deterministic interpreter, not the model, produces the final value (the PAL move).
3. **(Apply)** Run a self-consistency check: sample N reasoning paths, extract the final answers, and take the majority vote as the validated output.
4. **(Evaluate)** Given a reasoning output, choose the appropriate verification mechanism — proof/type checker, executor, self-consistency, PRM, or human — using the ground-truth-availability decision tree.
5. **(Evaluate)** Identify the cell where no mechanical validator exists and state honestly what cannot be certified there.

## Prerequisites

Ch. 1 (Why Validating AI Output Is a Different Problem) — fluency is not accuracy; verification is harder than generation. Ch. 2 (The Deterministic Floor) — external, deterministic feedback beats model judgment; this chapter applies that principle to reasoning by asking *when an oracle exists*. Ch. 4 (Validating Factual Claims) — the open-ended cell here hands off to factual and human validation. Forward pointers: Ch. 8 (LLM-as-judge) and Ch. 9 (Process Reward Models) are the evaluators this chapter previews.

---

## 5.1 The chain that graded itself

The following is a composite, assembled from documented self-correction patterns and labeled as such. The mechanism is real — every effect below is reproduced in the cited literature; the specific team is not.

A team building a quantitative-reasoning assistant shipped a "reflection" feature they were proud of. The model would solve a word problem, then re-read its own solution under the instruction *"Review your reasoning above. Identify any errors and correct them."* In their demos it looked like magic: the model would catch a dropped negative sign, rewrite the step, and arrive at the right answer. They reported the reflection step as a validation layer — "the model checks its work before answering" — and the phrase went into the product copy.

When they finally ran a controlled evaluation — same problems, with and without the reflection step, scored against known answers — the result was not what the demos promised. On the problems the model got right the first time, reflection sometimes talked it *out* of the correct answer: it would find an "error" that was not an error, rewrite a sound step, and land on a wrong total. Net accuracy did not improve. On the hardest subset it went *down*. The reflection step was not validating anything. It was resampling the model's own judgment and calling the second sample a check.

The team had made no reasoning error of their own. They had made a **validation** error, and it is among the most common in the field: they assumed that asking a model to find its mistakes gives it the ability to find its mistakes. It does not, because to correct an error you need a signal that points away from it — and the model's only signal is the same plausibility judgment that produced the error in the first place. This chapter is about where that signal *does* exist (and is mechanical and sound), where it does not, and what to do in each case. The book's thesis lands here with unusual sharpness: **validation works where ground truth is mechanically available and fails where it isn't.** Reasoning is the output type that makes the boundary visible, because for the same problem you can sometimes have a perfect oracle and sometimes have nothing at all.

---

## 5.2 Why ungrounded self-correction fails

Start with the negative result, because it dislodges the intuition the whole chapter is built to break. Huang et al. (2024), "Large Language Models Cannot Self-Correct Reasoning Yet" (ICLR 2024, arXiv:2310.01798), studied *intrinsic* self-correction: the model revises its own answer using only its own capabilities, with no external feedback, no tool, no ground-truth signal. The finding is blunt. On reasoning tasks, intrinsic self-correction does not reliably improve performance, and it sometimes degrades it. Prompting a model to "review and fix" its reasoning is not a free validation layer; on average it is a coin flip that occasionally costs you a correct answer.

The mechanism is worth stating precisely, because it generalizes. A correction step needs a *gradient* — some signal indicating which direction is "more correct." When you run unit tests, the gradient is the test result: pass or fail, externally and deterministically. When you run a compiler, the gradient is the type error. When a model critiques its own reasoning with nothing external, the only gradient available is its internal estimate of plausibility. But plausibility is exactly what generated the original answer. If the first pass produced a fluent-but-wrong chain, the model already found that chain plausible — so re-asking it to judge plausibility cannot reliably move away from the error. Worse, it can move *toward* a new error by "fixing" steps that were correct but looked unusual.

> **Misconception to dislodge.** "The model checked its work, so the answer is more trustworthy." A model re-reading its own output with no external signal is not checking; it is sampling its own judgment a second time. Two samples of a biased estimator do not cancel the bias. Validation requires a signal from *outside* the thing being validated — a test, an interpreter, a proof checker, or the agreement of *independent* samples. Self-critique alone supplies none of these.

This is not a claim that self-correction is useless in general. It is a claim about *ungrounded* self-correction on reasoning. Huang et al. are careful, and so should you be: when correction is grounded in external feedback — the model fixes its code after seeing the failing test, or revises its proof after the checker rejects it — that is a different mechanism, and it works, because the gradient is real. Part of what Huang et al. argue is that earlier literature conflated the two, attributing to "the model correcting itself" gains that actually came from the tool or the test. Hold the distinction firmly: the question is never "did the model reconsider?" It is "what external signal did the reconsideration condition on?" No external signal, no validation.

---

## 5.3 The strongest case: a deterministic oracle exists

When a deterministic oracle exists, reasoning validation is not merely good — it is *sound*. There are two flavors.

### Execution: make the interpreter the oracle (PAL)

Gao et al. (2023), "PAL: Program-aided Language Models" (ICML 2023, arXiv:2211.10435), diagnosed a specific and reproducible failure: large models *decompose* problems well — they correctly figure out what to compute — but they make arithmetic and logic mistakes while *executing* the computation in natural-language chain-of-thought. The remedy follows directly from the diagnosis. Let the model do the part it is good at (decomposition) and offload the part it is bad at (execution) to a runtime that cannot make arithmetic errors. The model emits a *program* as its reasoning chain; a Python interpreter runs it; the interpreter's output is the answer.

Consider a word problem: *"A bakery sold 3 dozen muffins in the morning and 17 in the afternoon, then received a delivery of 50 more. If they started with 12, how many do they have now?"* A natural-language chain might say "3 dozen is 36, plus 17 is 53, started with 12 so 65 sold, delivery 50…" — and somewhere in that prose the model can transpose a digit or lose track of a sign. The PAL version emits:

```python
morning = 3 * 12       # 3 dozen
afternoon = 17
sold = morning + afternoon
start = 12
delivery = 50
remaining = start - sold + delivery
print(remaining)
```

Whatever the model's mental arithmetic would have been, it is now irrelevant: `remaining` is computed by the interpreter, deterministically, every time. The interpreter is the ground-truth oracle for the computation. The validation question shifts from "did the model add correctly?" (which you cannot check without redoing the arithmetic) to "did the model translate the problem into the right program?" — and the program is short, inspectable, and re-runnable. You have not eliminated error; you have *moved* it to a layer where you can see it and the runtime catches the rest.

### Proof: send it to a checker

The purest case is formal proof. In a proof assistant — Lean, Coq, Isabelle — a proof is a formal object, and the checker either accepts it or rejects it. There is no "looks plausible." There is type-checks or does-not-type-check, a sound binary verdict. This is the archetype of *ground truth is mechanically available*, and it is old: in 1967 the Dutch mathematician **Nicolaas Govert de Bruijn** built **Automath**, widely considered the first proof checker — a machine that read a proof and accepted each step only if it followed by the rules, nothing taken on trust. By the mid-1970s an entire analysis textbook (Landau's *Grundlagen*) had been formally verified in it. The theoretical reason step-by-step checking *works* traces to **Gerhard Gentzen**'s natural deduction and cut-elimination (1934–35): inference reduces to local, mechanically-verifiable rules. The modern engineering descendant is **Leslie Lamport**'s TLA+, which brings mechanical proof and model-checking to working systems, not just pure math.

The catch is the **auto-formalization gap**. The oracle is perfect, but getting your reasoning *into* the oracle's language — translating a natural-language argument into Lean, or a real computation into a faithful program — is itself unreliable and is currently the bottleneck on extending deterministic validation. The oracle does not lie; the translator does.

> **Misconception to dislodge.** "If I have a proof checker / interpreter, my validation is complete." The oracle validates only what reaches it faithfully. A program can be valid and run cleanly while solving the wrong problem; a Lean proof can verify a theorem that is not the one you meant to state. The deterministic layer certifies *the formalized claim*, not *your intent*. Inspect the translation, not just the verdict.

---

## 5.4 No oracle, comparable answer: self-consistency

Most reasoning you will validate is not a formal proof and cannot be reduced to a short program. But a large and useful subset has a weaker property: the *final answer* is comparable across runs — it is a number, a label, a discrete choice. For that subset there is a judgment-free validator that does not require any oracle at all.

Wang et al. (2022), "Self-Consistency Improves Chain of Thought Reasoning in Language Models" (ICLR 2023, arXiv:2203.11171), replace greedy single-path decoding with a simple aggregation: sample a *diverse set* of reasoning paths from the model, then take the **majority-vote answer** across them, marginalizing over the paths. The intuition is that a hard problem has many valid routes to the correct answer but errors tend to be idiosyncratic — different wrong chains go wrong in different directions, while correct chains converge on the same destination. Agreement across *independently sampled trajectories* is therefore a signal that correlates with correctness, and crucially it requires no model self-assessment. Nobody grades anything; you just count.

The reported gains over plain chain-of-thought were large for the era: roughly +17.9 points on GSM8K, +11.0 on SVAMP, +12.2 on AQuA, +6.4 on StrategyQA, +3.9 on ARC-challenge. Treat these as historical, model- and era-specific deltas, not current state of the art — the lesson is the *mechanism*, which has aged well even as the numbers have aged.

In practice the procedure is short:

```python
from collections import Counter

def self_consistent_answer(model, prompt, n=20, temperature=0.7):
    samples = [model.generate(prompt, temperature=temperature) for _ in range(n)]
    answers = [extract_final_answer(s) for s in samples]   # parse the boxed/last number or label
    votes = Counter(answers)
    answer, count = votes.most_common(1)[0]
    return answer, count / n   # the vote share is a cheap confidence signal
```

The vote share (`count / n`) is a usable, if crude, confidence reading: a 19-of-20 consensus is a very different situation from a 6-5-4-3-2 split, and you can route low-consensus cases to a human. Note what makes this validation rather than mere ensembling: the samples must be *independent* (raised temperature, no shared scratchpad), and the answer must be *comparable* (you can tell when two samples agree). Where those hold, you get robustness for the price of N forward passes and no judgment.

But name the failure boundary, because self-consistency has a real one. Voting assumes the model's errors are *noise* — random, idiosyncratic, canceling. When the model is *systematically* biased — when most independent samples make the *same* wrong move because the error is baked into the model's prior, not sampled fresh each time — voting launders the popular wrong answer into a confident wrong answer. Self-consistency raises confidence in the majority; it does not check whether the majority is right. Where the bias is shared, more votes make you more sure and no more correct. This is precisely the cell where you want a real oracle and don't have one.

> **Misconception to dislodge.** "Twenty samples agreed, so the answer is validated." Agreement validates only against *noise*. If the error is systematic — shared across samples because it lives in the model's prior — twenty agreeing samples are twenty instances of the same mistake. Self-consistency is a robustness move, not a correctness proof; it is strongest exactly where it is least needed (easy problems with high agreement) and weakest where you need it most (hard problems where the model is confidently and consistently wrong).

---

## 5.5 The decision: verify by domain

Put the mechanisms in order. The choice of validator is not a matter of taste; it is dictated by one question asked repeatedly — *is ground truth mechanically available, and at what granularity?*

1. **Can the conclusion be checked by a deterministic oracle?**
   - **Yes, and it is a formal proof →** use a **proof/type checker** (Lean/Coq/Isabelle). Sound yes/no. Strongest validation that exists; bottleneck is faithful formalization.
   - **Yes, and it is computation →** express the reasoning as code and **execute** it (PAL). The interpreter is the oracle; inspect the translation, not the arithmetic.
2. **No oracle — is the final answer comparable across independent samples?**
   - **Yes →** use **self-consistency** (sample N, majority vote). Judgment-free robustness; watch for systematic (shared) bias.
3. **No comparable final answer — are intermediate steps labelable as correct/incorrect?**
   - **Yes →** use a **Process Reward Model** for step-level scoring (Ch. 9). Lightman et al. (2023), "Let's Verify Step by Step" (arXiv:2305.20050), showed process supervision beats outcome supervision on MATH — feedback on *each step* trains a stronger verifier than feedback on the final answer alone. PRMs are previewed here and treated fully in Ch. 9.
   - **No →** **human review.** This is the cell where validation is weakest. Mark it explicitly.

There is one branch that is always forbidden, at every leaf: **model self-critique alone** — asking the model to grade its own reasoning with no external signal (§5.2, Huang et al.). Picture the decision tree with that branch drawn and crossed out. The structural point of the whole tree is that *every legitimate leaf grounds in something external* — an interpreter, a checker, the agreement of independent samples, a step label, a human. The only path with no external grounding is ungrounded introspection, and it is the one path the evidence rules out.

Notice that step 3's question — "are intermediate steps labelable?" — is itself a ground-truth-availability test. PRMs presuppose that a human or model can mark an intermediate step as correct or incorrect. In math and code that label is usually well-defined (a step is correct if it preserves the truth of the derivation). In open-ended legal, strategic, or commonsense reasoning it often is not: there may be no fact of the matter about whether an intermediate inference is "correct," only whether the whole argument is persuasive. Where the step label is undefined, PRMs have nothing to score, and you are genuinely in the weakest cell — no oracle, no comparable answer, no labelable step. That cell is not a gap in our techniques; it is a gap in *ground truth*, and pretending a technique fills it is the failure the book exists to prevent.

---

## 5.6 The same problem, three ways

To make the escalation concrete, take one GSM8K-style problem and validate it three ways, weakest to strongest.

- **Single greedy chain (no validation).** Ask once, temperature 0, take the natural-language answer. Fragile: one dropped sign and the answer is wrong with no signal that anything failed. This is the baseline the rest improves on.
- **Self-consistency (statistical, judgment-free).** Sample 20 chains at temperature 0.7, majority-vote the final number. Robust to idiosyncratic arithmetic slips — they scatter; the right answer concentrates. Still blind to systematic bias, and gives you a confidence reading (vote share) for free. No oracle required.
- **PAL execution (deterministic oracle).** Have the model emit a Python program and run it. The arithmetic error class is *gone* — eliminated, not averaged down — because the interpreter computes the value. Your remaining risk is mistranslation, which is inspectable in a way that mental arithmetic is not.

The ladder is the lesson: from no validation, to statistical agreement, to a sound oracle. You climb as high as ground truth lets you. For this problem you can reach the top rung because the reasoning is computational. For a question like *"What is the strongest legal argument against this clause?"* there is no top rung — no interpreter, no proof checker, and arguably no comparable "final answer" to vote on — and the honest move is to say so and route to a human, not to dress up self-critique as a check.

---

## Exercises

1. **(Understand / Analyze)** A colleague's pipeline does: generate solution → prompt the model *"Find and fix any errors above"* → return the revised solution, and labels the second step "automated verification." (a) Name the precise mechanism by which this can *reduce* accuracy, citing Huang et al. (2024). (b) State what would have to be added to the second step to turn it from resampling into genuine validation. (c) Give one task type where the colleague's setup would actually help and explain why the gradient is real there.

2. **(Apply, produce-something)** Take ten arithmetic word problems with known answers. Implement two validators: (i) self-consistency with N=20 majority vote, and (ii) PAL — have the model emit a Python program and execute it. Produce a small table: per-problem, the single-greedy answer, the self-consistency answer with vote share, and the PAL answer. Write one paragraph on which problems the three methods disagreed on and why — and identify any problem where self-consistency confidently agreed on a *wrong* answer (systematic bias).

3. **(Evaluate)** For each of the following reasoning outputs, walk the §5.5 decision tree and name the validator you would use and what it cannot catch: (a) "Prove that the sum of the first n odd numbers is n²"; (b) "Compute the total interest on this amortization schedule"; (c) "Classify which of these 200 support tickets are billing-related"; (d) "Argue whether this acquisition is strategically wise." For (d), state explicitly what cannot be certified and who owns the residual risk.

4. **(Evaluate)** Self-consistency "launders systematic bias." Construct a concrete example: a problem where you would *expect* most independent samples to converge on the same wrong answer (e.g., a question with a tempting-but-incorrect intuitive answer). Predict the vote distribution, then run it if you can. Explain why adding more samples would not help, and what kind of validator *would*.

---

## What would change my mind

A robust demonstration that, for current frontier reasoning models, intrinsic self-correction — re-reading and revising with *no* external feedback — reliably improves reasoning accuracy across a broad task distribution, with the gain surviving controlled evaluation (with-vs-without, scored against ground truth) rather than appearing only in cherry-picked demos. The chapter's central claim is that ungrounded self-critique has no correctness gradient and so cannot reliably validate; if newer models internalized a genuinely independent error signal — such that the "review" pass conditions on something the first pass did not — the §5.2 prohibition would soften from "forbidden" to "use with measured expectations." I would also revise the open-ended-cell pessimism if step-level correctness in non-formal domains (legal, strategic) turned out to be reliably labelable by some method, which would extend PRMs into the cell I currently mark as human-only. Both are live research questions; neither is settled as of this writing. `[verify: status of intrinsic self-correction on frontier reasoning models as of reader's date]`

## Still puzzling

- **Defining step-correctness outside math and code.** PRMs need a correct/incorrect label per step. For legal, strategic, and commonsense reasoning that label may not exist — only "is the whole argument persuasive?" Whether step-level supervision is even *meaningful* there is open.
- **When voting launders systematic bias.** Self-consistency assumes errors are noise. The boundary between "idiosyncratic noise that cancels" and "shared bias that votes itself confident" is under-characterized, and you usually cannot tell which regime you are in from the vote distribution alone.
- **The auto-formalization gap.** The oracles (Lean, interpreters) are sound; translating natural-language reasoning into them faithfully is not. This translator, not the checker, is the current ceiling on how far deterministic reasoning-validation can reach.
- **Internalized verification in "thinking" models.** As reasoning models fold sampling and checking inside the forward pass, it gets harder to externally audit *which* validation actually fired — or whether the visible chain reflects the computation that produced the answer at all.

---

## References

- Wang, X., Wei, J., Schuurmans, D., Le, Q., Chi, E., Narang, S., Chowdhery, A., & Zhou, D. (2022). [Self-Consistency Improves Chain of Thought Reasoning in Language Models](https://arxiv.org/abs/2203.11171). *ICLR 2023.* arXiv:2203.11171.
- Huang, J., Chen, X., Mishra, S., Zheng, H. S., Yu, A. W., Song, X., & Zhou, D. (2023). [Large Language Models Cannot Self-Correct Reasoning Yet](https://arxiv.org/abs/2310.01798). *ICLR 2024.* arXiv:2310.01798.
- Gao, L., Madaan, A., Zhou, S., Alon, U., Liu, P., Yang, Y., Callan, J., & Neubig, G. (2022). [PAL: Program-aided Language Models](https://arxiv.org/abs/2211.10435). *ICML 2023.* arXiv:2211.10435.
- Lightman, H., Kosaraju, V., Burda, Y., Edwards, H., Baker, B., Lee, T., Leike, J., Schulman, J., Sutskever, I., & Cobbe, K. (2023). [Let's Verify Step by Step](https://arxiv.org/abs/2305.20050). arXiv:2305.20050. (Previewed here; full treatment in Ch. 9.)
- de Bruijn, N. G. (1970). The mathematical language AUTOMATH, its usage, and some of its extensions. *Symposium on Automatic Demonstration*, Springer LNM 125. (First proof checker; historical anchor.)
- Gentzen, G. (1935). Untersuchungen über das logische Schließen. *Mathematische Zeitschrift* 39. (Natural deduction; cut-elimination — the basis for mechanical step-by-step proof checking.)
- Lamport, L. (2002). *Specifying Systems: The TLA+ Language and Tools for Hardware and Software Engineers.* Addison-Wesley. (Mechanical proof/model-checking for engineering practice.)

---

**Tags:** reasoning-validation, self-consistency, intrinsic-self-correction, PAL, program-aided, proof-checker, lean-coq, process-reward-model, deterministic-oracle, verify-by-domain, ground-truth-availability
