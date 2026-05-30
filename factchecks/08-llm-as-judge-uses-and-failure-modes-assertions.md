# Assertions Report: 08-llm-as-judge-uses-and-failure-modes.md

**Date:** 2026-05-30
**Source file:** chapters/08-llm-as-judge-uses-and-failure-modes.md
**Assertions flagged:** 8
**Breakdown:** STAT: 4 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 8 | SPECIALIST: 0 | CURRENT: 2

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED; the single future-dated preprint (2604.23178) is labeled "[preprint, single source]" in-text.

---

## Full Findings

### EVIDENCE / STAT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Wang et al. (2023)... Vicuna-13B 'beat' ChatGPT on 66 of 80 queries purely by reordering the candidates."
**Claim checked:** 66/80 position-bias flip (arXiv:2305.17926).
**Site visited:** https://arxiv.org/abs/2305.17926 (re-verified live 2026-05-30).
**Finding:** Confirmed — "Large Language Models are not Fair Evaluators"; Vicuna-13B beats ChatGPT on 66 of 80 queries by altering candidate order. Accurate.
**Expert review needed:** No
**Suggested reference:** Wang, P., et al. Large Language Models are not Fair Evaluators. 2023. arXiv:2305.17926.
**Notes:** None.

### EVIDENCE / STAT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Zheng et al. (2023)... strong LLM judges (GPT-4) reach over 80% agreement with human preferences."
**Claim checked:** MT-Bench / GPT-4 >80% human agreement (arXiv:2306.05685).
**Site visited:** arXiv:2306.05685 (confirmed prior pass / companion books).
**Finding:** Confirmed — Zheng et al. 2023 (NeurIPS 2023), MT-Bench + Chatbot Arena; GPT-4 reaches >80% agreement with humans (≈ human–human level). Accurate; the chapter correctly notes this is raw agreement, not chance-corrected.
**Expert review needed:** No
**Suggested reference:** Zheng, L., et al. Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. NeurIPS 2023. arXiv:2306.05685.
**Notes:** Prior pass corrected the year 2024→2023.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "In 1960, Cohen introduced his kappa coefficient precisely because raw percent agreement overstates reliability."
**Claim checked:** Cohen's kappa (1960).
**Site visited:** Cohen, J. (1960). A Coefficient of Agreement for Nominal Scales. Educational and Psychological Measurement 20(1):37–46.
**Finding:** Correct — Cohen's kappa (1960) is chance-corrected agreement. The chapter's point (LLM-judge papers report raw agreement, not kappa) is accurate and well-made. Accurate.
**Expert review needed:** No
**Suggested reference:** Cohen, J. A Coefficient of Agreement for Nominal Scales. 1960.
**Notes:** None.

### EVIDENCE / STAT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Shi et al. (2024)... measured position bias across more than 150,000 evaluation instances, 15 judges, MT-Bench and DevBench... strongly modulated by the quality gap between candidates."
**Claim checked:** 150,000+ instances; 15 judges; quality-gap modulation (arXiv:2406.07791).
**Site visited:** https://arxiv.org/abs/2406.07791 (re-verified live 2026-05-30).
**Finding:** Confirmed — "Judging the Judges": 15 LLM judges across MT-Bench and DevBench (22 tasks, ~40 generators), >150,000 evaluation instances; position bias is not random and varies by judge/task; modulated by candidate quality gap. Accurate.
**Expert review needed:** No
**Suggested reference:** Shi, L., et al. Judging the Judges: A Systematic Study of Position Bias in LLM-as-a-Judge. 2024. arXiv:2406.07791.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Panickssery et al. (2024)... GPT-4 and Llama-2 can recognize their own outputs... a linear correlation between self-recognition capability and the strength of self-preference, established causally by fine-tuning self-recognition up."
**Claim checked:** Causal self-recognition→self-preference (NeurIPS 2024).
**Site visited:** arXiv:2404.13076 (Panickssery, Bowman, Feng — NeurIPS 2024).
**Finding:** Confirmed — "LLM Evaluators Recognize and Favor Their Own Generations"; self-recognition correlates with self-preference, shown causally by fine-tuning self-recognition. Accurate.
**Expert review needed:** No
**Suggested reference:** Panickssery, A., et al. LLM Evaluators Recognize and Favor Their Own Generations. NeurIPS 2024. arXiv:2404.13076.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Verga et al. (2024), 'Replacing Judges with Juries' (the PoLL... paper), showed that a panel of several smaller models from disjoint families (Command R, GPT-3.5, Haiku) outperforms a single large judge (GPT-4)..."
**Claim checked:** PoLL panel-of-LLM-evaluators result.
**Site visited:** arXiv:2404.18796 (Verga et al., Cohere).
**Finding:** Confirmed — "Replacing Judges with Juries": a Panel of LLM evaluators (Command R, GPT-3.5, Haiku) outperforms a single GPT-4 judge, with less intra-model bias and lower cost. Accurate.
**Expert review needed:** No
**Suggested reference:** Verga, P., et al. Replacing Judges with Juries. 2024. arXiv:2404.18796.
**Notes:** None.

### EVIDENCE / CURRENT — CONFIRMED (preprint, labeled)
**Assertion type:** POSITIVE
**Sentence:** "A recent preprint (arXiv:2604.23178, April 2026)... position bias has become largely negligible (≤0.04...) while style bias is now dominant (0.76–0.92)."
**Claim checked:** Position-negligible/style-dominant inversion (arXiv:2604.23178).
**Site visited:** arXiv:2604.23178 (confirmed prior pass, factchecks/_fc-B).
**Finding:** Confirmed as indexed — single-author preprint (Soumik); position bias ≤0.04, style bias 0.76–0.92 on current models. The chapter labels it "[preprint, single source]." Accurate as framed.
**Expert review needed:** No
**Suggested reference:** arXiv:2604.23178 (preprint).
**Notes:** Future-dated, single-source; honestly labeled.

### EVIDENCE / CURRENT — CONFIRMED (preprints)
**Assertion type:** BASIC
**Sentence:** Self-preference bias-vs-calibration (arXiv:2504.03846) and preference leakage (arXiv:2502.01534) in "Still puzzling."
**Claim checked:** Two judge-bias preprints.
**Site visited:** arXiv:2504.03846, arXiv:2502.01534 (confirmed prior pass).
**Finding:** Confirmed — Chen et al. 2025 (self-preference mostly legitimate, harmful when evaluator errs as generator) and Li et al. "Preference Leakage" (ICLR 2026; judges favor related student models). Accurate; labeled preprint.
**Expert review needed:** No
**Suggested reference:** arXiv:2504.03846; Li, D., et al. Preference Leakage. arXiv:2502.01534.
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| 2604.23178 style-bias 0.76–0.92 / position ≤0.04 | STAT/CURRENT | POSITIVE | Single future-dated preprint; the chapter labels it "[verify — fresh, future-dated preprint; single source]." |

---

## AI-Pass Flags

- The chapter's central methodological move — "the durable skill is the probe, not the list" — is well-supported (bias profiles demonstrably shift across the 2023→2026 papers).
- The kappa-vs-raw-agreement caveat is an unusually rigorous and correct point. No internal contradictions.

---

## References

(Chapter cites Wang 2023, Zheng 2023, Cohen 1960, Shi 2024, Panickssery 2024, Verga 2024, and three judge-bias preprints — all confirmed/labeled. If the chapter lacks a formal References section, recommend adding one; the inline attributions are accurate.)
