> **Voice status:** `voice-unanchored`. Root `style/` and book-level `style/` empty as of this draft.

---

# Chapter 8 — LLM-as-Judge: Uses and Failure Modes

*A fluent model judging a fluent model can share the very blind spot it was hired to catch*

**Author:** Nik Bear Brown
**Editor:** Nik Bear Brown

---

## Suggested titles

1. **LLM-as-Judge: Screen With It, Never Certify With It**
2. **The Same Two Answers, Reordered, Flip the Winner: Testing for Judge Bias**
3. **When the Evaluator Shares the Generator's Blind Spot: The Circularity Problem**

---

## TL;DR

When ground truth is mechanically available you use a deterministic validator (Ch. 2). The LLM judge is what you reach for when it *isn't* — open-ended quality, helpfulness, "which answer is better." And in that exact regime the evaluator is another fluent model, so it inherits the human reviewer's fluency-as-proxy weakness *and* a new one: via self-recognition it can share the generator's blind spots, rating a wrong answer highly because it is wrong in the judge's own style. The paradigm is real and useful: Zheng et al. (2023, NeurIPS 2023, arXiv:2306.05685) introduced MT-Bench and showed strong judges (GPT-4) reach **over 80% agreement with human preferences — the same level humans reach with each other.** The same paper catalogs the failure modes. The canonical demonstration: Wang et al. (2023, arXiv:2305.17926) flipped a judge's verdict by *reordering the two candidates* — Vicuna "beat" ChatGPT on 66/80 queries purely by order. Shi et al. (2024, arXiv:2406.07791) measured position bias across >150,000 instances and found it is **strongly modulated by the quality gap** — closer calls flip more — which is the mechanism behind the chapter's rule: **judges are fine for coarse ranking and screening, untrustworthy near the margin.** Self-preference is causal, not coincidental: Panickssery et al. (2024, arXiv:2404.13076) tie a judge's self-preference linearly to its ability to recognize its own outputs. The durable skill is *not memorizing a bias list* — bias profiles shift with model generation (a 2026 preprint, arXiv:2604.23178, reports position bias now negligible (≤0.04) while style bias dominates (0.76–0.92) `[verify — fresh, future-dated]`). The durable skill is **running the probe**: swap order, pad length, score self-vs-other, and measure the flip rate. Diverse juries (PoLL; Verga et al. 2024, arXiv:2404.18796) beat single large judges and *partially* break circularity — partially, because shared training lineage means a panel can still share blind spots.

---

## Learning objectives

By the end of this chapter you should be able to:

1. **(Understand)** Explain why an LLM judge is invoked precisely where mechanical ground truth is absent, and why that places the evaluator's own biases and blind spots inside the validation.
2. **(Evaluate)** Decide when an LLM judge is an appropriate instrument (coarse ranking, recoverable-error screening) and when it must be refused (close calls), using the quality-gap mechanism.
3. **(Apply)** Design and run a bias *probe* for position, verbosity, self-preference, and style — measuring a flip or score-lift rate rather than asserting a bias from a fixed list.
4. **(Evaluate)** Diagnose the circularity / shared-failure-mode problem and assess whether a diverse jury (PoLL) genuinely mitigates it or merely averages a shared blind spot.
5. **(Evaluate)** Read a reported "judge–human agreement" figure and state what it does and does not certify, including the chance-correction subtlety.

## Prerequisites

Ch. 2 (The Deterministic Floor) — the judge is the tool you reach for *after* deterministic ground truth is exhausted, never before it. Ch. 5 (Validating Reasoning Chains) — the self-critique-is-weak result is the single-model version of this chapter's circularity. Ch. 7 (Validating Agentic Task Execution) — Agent-as-a-Judge is this chapter's problem with actions attached; the circularity warning there is completed here. This chapter feeds Ch. 9 (Process Reward Models), the step-level alternative when the judge is too coarse.

---

## 8.1 The same two answers, reordered, flipped the winner

The following is a composite of documented LLM-judge behavior, labeled as such — but the central demonstration is not hypothetical; it is the reproducible result of Wang et al. (2023), restated.

A team building a customer-support assistant wanted to choose between two candidate response generators, A and B. They had no labeled ground truth for "which reply is more helpful" — the whole point of the feature was open-ended quality, the kind of thing a schema or a test cannot score. So they did the now-standard thing: they used a strong LLM as a judge. For each of 80 support scenarios, they showed the judge the prompt and the two replies and asked which was better. B won, comfortably — 58 of 80. They wrote it up: *Generator B is the better model; ship B.*

Then a skeptic on the team ran one extra experiment. She presented the judge with the *identical* 80 pairs, but swapped the order — B first, A second, instead of A first, B second. The verdict moved. On a large fraction of the pairs where B had "won," A now won, for no reason but the swap. The two answers were the same. The content was the same. The only thing that changed was which response the judge read first. The "B is better" result was not a measurement of quality; it was, in substantial part, a measurement of *position*.

This is Wang et al. (2023), "Large Language Models are not Fair Evaluators," in miniature: with ChatGPT as evaluator, Vicuna-13B "beat" ChatGPT on **66 of 80** queries purely by reordering the candidates. The team made no generation error and no data error. They made a **validation** error — they treated a judge's verdict as if it measured the thing they cared about, when it partly measured an artifact of presentation. And it is the cleanest possible illustration of this book's thesis. **Validation fails where ground truth isn't mechanically available** — they used a judge *because* there was no oracle for "more helpful" — and **the evaluator itself can be biased**, so the judge introduced an error of its own that looked exactly like a finding.

This chapter is about using the judge anyway, because it is genuinely useful, while knowing precisely where it lies to you — and, crucially, learning to *test* a given judge for bias rather than memorizing which biases are currently fashionable, because the profile changes with every model generation.

---

## 8.2 What the judge is good at — and the agreement number's fine print

Start with the legitimate case, because the judge is not a bad instrument; it is a misused one. Zheng et al. (2023), "Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena" (NeurIPS 2023, arXiv:2306.05685), is the origin paper. It introduced MT-Bench (80 multi-turn questions), analyzed Chatbot Arena crowd data, and reported the headline that launched the paradigm: **strong LLM judges (GPT-4) reach over 80% agreement with human preferences — the same level of agreement humans reach with one another.** (Note the date: posted 2023, presented at NeurIPS 2023; cite as 2023, not 2024.) That is a real, reproducible result, and it licenses real uses:

- **Coarse ranking and leaderboards** — separating clearly-better from clearly-worse candidates across many comparisons.
- **Recoverable-error screening** — a cheap first-pass filter ("is this answer grounded? on-topic? safe?") where a false negative costs a re-check, not a catastrophe.
- **Scale** — judging thousands of outputs no human team could read.

But the ">80% agreement" number has fine print the field routinely ignores, and it is **Jacob Cohen**'s fine print. In 1960, Cohen introduced his kappa coefficient precisely because *raw percent agreement* overstates reliability: two raters who agree 80% of the time may be only marginally better than two raters guessing, if the base rates make 80% easy to hit by chance. The honest reliability statistic is *chance-corrected* — agreement beyond what coincidence predicts. Almost no LLM-judge paper reports kappa; they report raw agreement. So treat "~80% judge–human agreement" as the *ceiling-flavored* number it is — encouraging, real, and *not* the same as "80% reliable." The deeper lineage runs through the history of scientific peer review itself (institutionalized by **Henry Oldenburg** at the Royal Society in the 1660s): peer review is the original *fallible judge used to screen, not to certify*, complete with its own documented biases — prestige, position-in-the-pile, reviewer self-interest — that map almost one-to-one onto the LLM-judge biases below.

> **Misconception to dislodge.** "The judge agrees with humans 80% of the time, so it's 80% reliable." Raw agreement is not chance-corrected reliability (Cohen 1960). On a task where one answer is clearly better most of the time, 80% raw agreement can be barely above what flipping a coin would score. Use the agreement figure to justify *screening*, not certification — and ask for kappa before you trust a headline.

---

## 8.3 The decision pivot: coarse ranking is fine, close calls are not

The single most useful rule in this chapter comes from the *mechanism* of position bias, not from a warning. Shi et al. (2024), "Judging the Judges: A Systematic Study of Position Bias in LLM-as-a-Judge" (arXiv:2406.07791), measured position bias across **>150,000 evaluation instances, 15 judges, MT-Bench + DevBench**, with three metrics: repetition stability, position consistency, preference fairness. The finding that matters: position bias is **not random**, it varies by judge and task, it is only weakly tied to response length — and it is **strongly modulated by the quality gap between candidates.** The closer the true quality of the two answers, the more the verdict is driven by ordering rather than content.

That gives a clean, internalizable decision rule:

> **Use the judge for coarse separation; refuse it near the margin.** When one candidate is clearly better, the judge's verdict tracks quality and position bias is small. When the candidates are near-equal — the exact close call you most want resolved — position bias dominates and the judge is least trustworthy precisely where you need it most.

This is why the §8.1 team's "B wins 58–22" was unsafe: a 58–22 split over 80 noisy pairs likely contains many near-ties, and near-ties are where the swap flips the verdict. The pairwise A/B comparison of two near-equal candidates is the *canonical misuse* of an LLM judge. The appropriate uses — coarse ranking, screening where errors are recoverable — all live in the clearly-separated regime. The inappropriate use lives at the margin, and the margin is exactly where people most want a verdict.

> **Misconception to dislodge.** "If I just average over both orderings, the judge is fair." Balanced position calibration (averaging the verdict over both orders; Wang et al. 2023) genuinely reduces position bias and you should do it. But averaging a coin-flip is still uninformative: on a true close call, the two orderings disagree *because the judge cannot tell the answers apart*, and the average is a 50/50 that tells you nothing about quality. Calibration removes the *directional* artifact; it does not manufacture a signal the judge does not have.

---

## 8.4 The bias catalog — and why you test for it instead of memorizing it

There is a catalog of judge biases, and you should know it. But the chapter's actual lesson is one level up: **bias profiles change as models improve, so the durable skill is the probe, not the list.** TIKTOC's own aging audit flags this, and a recent preprint makes it vivid — arXiv:2604.23178 reports that on current-generation models, position bias has become *negligible* (≤0.04 across five tested models, attributed to better instruction tuning) while *style* bias is now dominant (0.76–0.92) and under-studied. `[verify — this is a fresh, future-dated (April 2026) preprint; treat as a single recent result, not settled literature.]` If you had memorized "position bias is the big one" from the 2023 papers, you would be testing for yesterday's bias and missing today's. So: learn the catalog as a set of *probes you can run*, each with a mechanism, a test, and a mitigation — and re-run the probes on every model you deploy.

| Bias | Mechanism | Detection probe (run this) | Mitigation | Status note |
|---|---|---|---|---|
| **Position** | Verdict tracks order, especially on close calls (Shi 2024) | Swap candidate order on the same pairs; measure the flip rate | Balanced position calibration (average both orders) | Largely negligible on frontier models per arXiv:2604.23178 `[verify]` |
| **Verbosity** | Longer ≈ better, quality held equal (Saito 2023, arXiv:2310.10076) | Pad one answer with on-topic filler; measure the score lift | Length-controlled scoring; rubric scoring | Persistent; propagates into RLAIF training |
| **Self-preference** | Judge recognizes and favors its own outputs (Panickssery 2024) | Score self-generated vs. other-generated answers of equal quality; measure the gap | Diverse jury (Verga); never self-as-judge | Persistent; the engine of circularity |
| **Style** | Format/tone scored over substance | Hold content fixed, vary formatting/tone; measure the score change | Substance-anchored rubric | **Dominant** per arXiv:2604.23178 `[verify]` |

The self-preference probe is the one that connects to the whole chapter's thesis, because Panickssery et al. (2024), "LLM Evaluators Recognize and Favor Their Own Generations" (NeurIPS 2024, arXiv:2404.13076), showed the bias is **causal, not coincidental**: GPT-4 and Llama-2 can recognize their own outputs with non-trivial accuracy, and there is a *linear correlation between self-recognition capability and the strength of self-preference* — established causally by fine-tuning self-recognition up and watching self-preference rise with it. A judge that can detect its own style favors it. That is not a quirk; it is a mechanism, and it is the bridge to circularity. (The framing is contested at the edge: arXiv:2504.03846 asks whether a model favoring its own outputs is *bias* or partly *legitimate calibration* — a model may genuinely produce better outputs in its own style. `[verify]` Hold the question open; the probe is the same either way.)

> **Misconception to dislodge.** "I read the bias papers, so I know which biases my judge has." You know which biases the *2023–2024 papers' models* had. Yours may differ — position bias may have closed; style bias may now dominate. The only way to know your judge's profile is to *probe your judge*: swap, pad, self-vs-other, hold-content-vary-style, and measure the rates. Teach yourself the test, not the list.

---

## 8.5 Circularity: when the evaluator shares the generator's blind spot

Here is the deepest failure, the one that makes LLM-as-judge categorically different from a deterministic validator. A compiler does not share a code generator's misconceptions; it is a different kind of thing. An LLM judge *is the same kind of thing as the generator* — a fluent model trained on overlapping data with overlapping objectives — so it can be **wrong in the same way the generator is wrong**, and rate a flawed answer highly *because* the flaw is one the judge also holds.

Panickssery's self-recognition mechanism is the engine: if the judge recognizes its own style and favors it, then a wrong-but-fluent answer in the judge's own idiom gets rated highly precisely because it *reads* like something the judge would produce. The validation becomes circular — the answer is approved for sharing the judge's blind spot, not for being correct. This is the book's thesis at its sharpest: when ground truth is gone, the evaluator is *another instance of the thing being evaluated*, and it can confirm rather than catch the shared error.

The standard mitigation is a **jury, not a judge**. Verga et al. (2024), "Replacing Judges with Juries" (the PoLL — Panel of LLm evaluators — paper, arXiv:2404.18796), showed that a panel of several *smaller* models from **disjoint families** (Command R, GPT-3.5, Haiku) outperforms a single large judge (GPT-4), exhibits **less intra-model bias**, and costs less. The logic is diversity-defeats-circularity: models from different families have different blind spots, so a blind spot in one is caught by another, and the panel's shared bias is smaller than any single member's.

But say the honest part. A jury *partially* breaks circularity, not fully. If the panel members share pretraining corpora, share RLHF lineage, or were tuned on the same human-preference data, they may share blind spots *that survive ensembling* — the panel does not catch the error; it averages a consensus around it. Diversity reduces shared failure to the extent the members are genuinely independent, and frontier models are less independent than their different names suggest (overlapping web-scale corpora, similar alignment recipes). There is no clean test for whether a given panel breaks circularity or merely averages it — which is itself an open problem. So: prefer a diverse jury to a single judge, *especially* never use the generator's own model as its judge — and still treat the jury's verdict as a strong screen, not a certification, for anything near the margin.

> **Misconception to dislodge.** "A panel of judges removes the bias." A panel reduces *intra-model* bias and partially breaks circularity (Verga 2024). It does not remove a bias the members *share* through common training data or alignment lineage — that bias survives the average. Diversity helps in proportion to genuine independence, and frontier models share more than their branding implies. The jury is a better screen, not an oracle.

---

## Exercises

1. **(Understand / Evaluate)** A team reports: *"We used GPT-4 as a judge to compare two summarizers; it preferred Summarizer B on 60 of 100 examples, so B is better."* (a) Name the specific experiment from §8.1 you would run *first* before trusting this, and what result would invalidate the conclusion. (b) Explain, using Shi et al.'s quality-gap mechanism, why a 60–40 split is more suspect than a 90–10 split. (c) State what claim, if any, this evaluation is licensed to make after you run the order-swap.

2. **(Apply, produce-something)** Build a four-probe bias test harness for one LLM judge of your choice on one task. Implement and run: (i) a **position** probe (swap order, report flip rate); (ii) a **verbosity** probe (pad one answer with on-topic filler, report score lift); (iii) a **self-preference** probe (score the judge's own output vs. another model's, equal quality, report the gap); (iv) a **style** probe (hold content fixed, vary formatting/tone, report score change). Produce a table — probe, metric, measured rate — and a one-paragraph verdict on which biases *your* judge actually exhibits, explicitly comparing your result to the 2023–2024 papers and noting any divergence.

3. **(Evaluate)** You must validate open-ended answer quality with no mechanical ground truth. Walk a decision tree: (a) Is mechanical ground truth *truly* unavailable, or did you skip a deterministic check (Ch. 2/6)? (b) Are errors cheap and recoverable (screening) or not? (c) Is it a close call (small quality gap)? For each branch, state whether you use a deterministic validator, a single judge with probes, a diverse jury, or human escalation — and justify the close-call branch using the position-bias mechanism.

4. **(Evaluate)** Critique this circularity-mitigation plan: *"To avoid self-preference, we'll judge our GPT-4-generated answers with a panel of GPT-4o, GPT-4-turbo, and GPT-3.5."* (a) Explain why this panel only partially breaks circularity and what it shares. (b) Propose a more independent panel and state the property you optimized for. (c) Explain why even a maximally diverse jury cannot be treated as certification for a close call, connecting to Cohen's chance-correction point in §8.2.

---

## What would change my mind

A standing, versioned benchmark that re-measures the full bias catalog on each new model release and shows that on current frontier judges *every* bias in the catalog — not just position — has fallen into the negligible range (say, all flip/lift rates within sampling noise), *and* that this holds on close calls, not just clear separations. If that held, the "refuse the judge for close calls" rule would weaken toward "judges are reliable across the quality-gap range," and the chapter would pivot from *testing for bias* to *monitoring for regression*. I would separately revise the circularity pessimism if a clean test emerged that could certify a given jury genuinely breaks shared blind spots (not merely averages them) — but the shared-training-lineage problem makes me expect circularity to persist as long as judges and generators are drawn from overlapping model populations. As of this writing, the bias profile is shifting (position down, style up, per one fresh preprint) but not gone, and circularity has no clean test. `[verify both against the reader's-date literature; arXiv:2604.23178 is future-dated and single-source.]`

## Still puzzling

- **Does diversity break circularity or only average it?** When panel members share pretraining corpora and RLHF lineage, shared blind spots may survive ensembling. No clean test exists.
- **Stability of bias profiles across generations.** The reported position-down/style-up inversion suggests any fixed bias list ages fast; we lack a standing, versioned benchmark that re-measures biases per model release.
- **Self-preference: bias or signal?** Whether a model favoring its own outputs is error or partial correctness is unresolved (arXiv:2504.03846, `[verify]`).
- **Preference leakage / contamination.** When judge and generator share data or are tuned on the same preferences, that is a distinct, under-measured circularity channel (arXiv:2502.01534, `[verify]`).
- **Chance-corrected agreement.** Reported judge–human agreement is rarely kappa-corrected (Cohen 1960), so headline ~80% figures may overstate reliability, and we lack a norm of reporting the corrected number.

---

## References

- Zheng, L., Chiang, W.-L., Sheng, Y., et al. (2023). [Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena](https://arxiv.org/abs/2306.05685). arXiv:2306.05685. *NeurIPS 2023 Datasets & Benchmarks.* (Origin paper; >80% GPT-4–human agreement; catalogs position/verbosity/self-enhancement biases. Date is 2023.)
- Wang, P., Li, L., Chen, L., et al. (2023). [Large Language Models are not Fair Evaluators](https://arxiv.org/abs/2305.17926). arXiv:2305.17926. (Position bias; reorder flips the verdict — Vicuna "beats" ChatGPT 66/80 by order. Balanced position calibration.)
- Shi, L., Ma, C., Liang, W., et al. (2024). [Judging the Judges: A Systematic Study of Position Bias in LLM-as-a-Judge](https://arxiv.org/abs/2406.07791). arXiv:2406.07791. (>150k instances; position bias strongly modulated by the quality gap — the close-call mechanism.)
- Panickssery, A., Bowman, S. R., & Feng, S. (2024). [LLM Evaluators Recognize and Favor Their Own Generations](https://arxiv.org/abs/2404.13076). arXiv:2404.13076. *NeurIPS 2024.* (Self-recognition causally linked to self-preference — the engine of circularity.)
- Verga, P., Hofstätter, S., Althammer, S., et al. (2024). [Replacing Judges with Juries: Evaluating LLM Generations with a Panel of Diverse Models (PoLL)](https://arxiv.org/abs/2404.18796). arXiv:2404.18796. (Diverse panel of smaller models beats a single large judge with less intra-model bias.)
- Saito, K., et al. (2023). [Verbosity Bias in Preference Labeling by Large Language Models](https://arxiv.org/abs/2310.10076). arXiv:2310.10076. (Judges prefer longer answers at equal quality; propagates into RLAIF.)
- Ye, J., et al. (2024). [Justice or Prejudice? Quantifying Biases in LLM-as-a-Judge](https://arxiv.org/abs/2410.02736). arXiv:2410.02736. (12-bias taxonomy; CALM framework.)
- *Judging the Judges: A Systematic Evaluation of Bias Mitigation Strategies in LLM-as-a-Judge Pipelines* (2026). [arXiv:2604.23178](https://arxiv.org/abs/2604.23178). (Position bias now negligible ≤0.04; style bias dominant 0.76–0.92, on current models. `[verify — fresh, future-dated preprint; single source.]`)
- Cohen, J. (1960). A coefficient of agreement for nominal scales. *Educational and Psychological Measurement* 20(1). (Chance-corrected agreement — why raw 80% agreement is not 80% reliability.)

---

**Tags:** llm-as-judge, mt-bench, position-bias, verbosity-bias, self-preference, style-bias, circularity, shared-blind-spot, quality-gap, close-calls, bias-probe, balanced-position-calibration, poll-jury, panel-of-judges, cohens-kappa, chance-corrected-agreement, test-the-judge-not-the-list
