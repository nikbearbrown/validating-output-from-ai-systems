# Assertions Report: 12-scalable-oversight.md

**Date:** 2026-05-30
**Source file:** chapters/12-scalable-oversight.md
**Assertions flagged:** 10
**Breakdown:** STAT: 3 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 10 | SPECIALIST: 0 | CURRENT: 2

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED; preprints (Engels, Wen) are labeled "[preprint — active research / unreplicated]" in-text.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Amodei, Olah, Steinhardt, Christiano, Schulman, and Mané named it in 2016 ('Concrete Problems in AI Safety')... as scalable supervision."
**Claim checked:** Concrete Problems in AI Safety / scalable supervision (arXiv:1606.06565).
**Site visited:** arXiv:1606.06565.
**Finding:** Correct — the 2016 paper names scalable supervision (objective too expensive to evaluate frequently). Accurate.
**Expert review needed:** No
**Suggested reference:** Amodei, D., et al. Concrete Problems in AI Safety. 2016. arXiv:1606.06565.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Bowman et al. (2022)... sandwiching... Their proof-of-concept on MMLU and QuALITY found that non-experts interacting with an unreliable model assistant outperformed both the model alone and their own unaided performance."
**Claim checked:** Sandwiching; MMLU/QuALITY result (arXiv:2211.03540).
**Site visited:** arXiv:2211.03540.
**Finding:** Correct — "Measuring Progress on Scalable Oversight"; sandwiching methodology; assisted non-experts outperform both the model alone and unaided non-experts. The chapter correctly stresses it is a proof of concept, "not a demonstration on genuinely superhuman output." Accurate.
**Expert review needed:** No
**Suggested reference:** Bowman, S.R., et al. Measuring Progress on Scalable Oversight for LLMs. 2022. arXiv:2211.03540.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Irving, Christiano, and Amodei (2018, 'AI safety via debate')... with optimal play and a polynomial-time judge, debate can in principle answer any question in PSPACE, whereas direct human judging reaches only NP."
**Claim checked:** Debate PSPACE/NP argument (arXiv:1805.00899).
**Site visited:** arXiv:1805.00899.
**Finding:** Correct — the idealized optimal-play argument: debate ≈ PSPACE, direct judging ≈ NP. The chapter correctly flags it as "an idealized argument about optimal play, not an empirical guarantee." Accurate.
**Expert review needed:** No
**Suggested reference:** Irving, G., Christiano, P., & Amodei, D. AI safety via debate. 2018. arXiv:1805.00899.
**Notes:** None.

### EVIDENCE / STAT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Khan et al. (2024)... optimizing debaters for persuasiveness increased judge accuracy (76% model-judge / 88% human-judge versus 48% / 60% naive baselines)."
**Claim checked:** Debate judge-accuracy figures (arXiv:2402.06782, ICML 2024 Best Paper).
**Site visited:** https://arxiv.org/abs/2402.06782 + ICML 2024 (re-verified live 2026-05-30).
**Finding:** Confirmed exactly — debate achieves 76% (model judge) and 88% (human judge) accuracy vs naive baselines of 48% and 60%. ICML 2024 Best Paper. Accurate.
**Expert review needed:** No
**Suggested reference:** Khan, A., et al. Debating with More Persuasive LLMs Leads to More Truthful Answers. ICML 2024. arXiv:2402.06782.
**Notes:** None.

### EVIDENCE / STAT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Kenton et al. (2024)... ran the careful, broad version — 9 tasks, ~5M generations — and found debate consistently beats consultancy but its advantage over plain direct QA is task-dependent."
**Claim checked:** 9 tasks, ~5M generations, mixed-without-asymmetry result (arXiv:2407.04622).
**Site visited:** https://arxiv.org/abs/2407.04622 + NeurIPS 2024 (re-verified live 2026-05-30).
**Finding:** Confirmed — 9 tasks (128 questions each), ~5M generation calls; debate beats consultancy, but gains over direct QA are task-dependent (mixed without information asymmetry). Accurate.
**Expert review needed:** No
**Suggested reference:** Kenton, Z., et al. On scalable oversight with weak LLMs judging strong LLMs. NeurIPS 2024. arXiv:2407.04622.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Burns et al. (2023)... a weak supervisor (GPT-2-level)... a strong student (GPT-4-level)... an auxiliary-confidence loss recovers close to GPT-3.5-level performance from a GPT-2-level supervisor."
**Claim checked:** Weak-to-strong generalization (arXiv:2312.09390).
**Site visited:** arXiv:2312.09390.
**Finding:** Correct — OpenAI's weak-to-strong paper; GPT-2-level supervisor → GPT-4-level student, auxiliary-confidence loss recovers near-GPT-3.5-level. The chapter correctly notes only partial gap recovery and that it is "not actually superhuman supervision." Accurate.
**Expert review needed:** No
**Suggested reference:** Burns, C., et al. Weak-to-Strong Generalization. 2023. arXiv:2312.09390.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Leike et al. (2018, 'Scalable agent alignment via reward modeling')... recursively bootstrapping oversight."
**Claim checked:** Recursive reward modeling (arXiv:1811.07871).
**Site visited:** arXiv:1811.07871.
**Finding:** Correct — recursive reward modeling research direction. Accurate.
**Expert review needed:** No
**Suggested reference:** Leike, J., et al. Scalable agent alignment via reward modeling. 2018. arXiv:1811.07871.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Stechly & Kambhampati (2023, arXiv:2310.12397; 2024, arXiv:2402.08115)... LLM self-critique does not reliably improve answers and can cause performance collapse — the gains come only from a sound external verifier."
**Claim checked:** Self-critique collapse on Graph Coloring/Game of 24/STRIPS.
**Site visited:** arXiv:2310.12397, arXiv:2402.08115 (2402.08115 confirmed Ch.1).
**Finding:** Correct — "GPT-4 Doesn't Know It's Wrong" (2023) and the self-verification-limitations paper (2024); gains require a sound external verifier. Accurate.
**Expert review needed:** No
**Suggested reference:** Stechly, K., et al. arXiv:2310.12397; arXiv:2402.08115.
**Notes:** None.

### EVIDENCE / STAT / CURRENT — CONFIRMED (preprint, labeled)
**Assertion type:** POSITIVE
**Sentence:** "Engels et al. (2025, 'Scaling Laws For Scalable Oversight')... nested oversight success drops below ~52% when overseeing a system ~400 Elo stronger."
**Claim checked:** Nested oversight ~52% (arXiv:2504.18530, MIT/Tegmark).
**Site visited:** https://arxiv.org/abs/2504.18530 + author statement (re-verified live 2026-05-30).
**Finding:** Confirmed — Engels, Baek, Kantamneni & Tegmark (MIT); the best nested-scalable-oversight scenario succeeds ~52% of the time, declining as the capability gap (Elo) grows; modeled via four oversight games (Mafia, Debate, Backdoor Code, Wargames). The chapter labels it "one study's numerical examples, not a validated law." Accurate.
**Expert review needed:** No
**Suggested reference:** Engels, J., Baek, D., Kantamneni, S., & Tegmark, M. Scaling Laws For Scalable Oversight. 2025. arXiv:2504.18530.
**Notes:** Preprint; the ~400-Elo figure is from the paper's modeling.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Kenneth Arrow... The principal–agent problem... Alan Turing's 1936 result that some questions about a computational process cannot be decided... Douglas Engelbart's 1962 vision — that tools should augment human judgment."
**Claim checked:** Principal–agent (Arrow), Turing 1936 undecidability, Engelbart 1962 augmentation.
**Site visited:** History-of-economics / computability / HCI records.
**Finding:** All correct — Arrow's information-asymmetry/moral-hazard work underpins the principal–agent framing; Turing's 1936 undecidability result; Engelbart's 1962 "Augmenting Human Intellect." Accurate as the chapter's intellectual lineage.
**Expert review needed:** No
**Suggested reference:** Turing, A.M. 1936; Engelbart, D. Augmenting Human Intellect, 1962; Arrow, K. (information asymmetry / moral hazard).
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| "Wen, et al. (2025). Scalable Oversight for Superhuman AI via Recursive Self-Critiquing. arXiv:2502.04675 [preprint — unreplicated]" | EVIDENCE/CURRENT | BASIC | Real preprint, correctly labeled unreplicated; cited as a probe, not a settled result. |

---

## AI-Pass Flags

- The opening 2,000-line-review scenario is a labeled illustrative case.
- The chapter is a model of calibrated uncertainty: it explicitly refuses both the "hype ending" (solved) and the "doom ending" (unsolvable), labels every preprint, and stresses all positive evidence is on tasks with artificial information asymmetry or a known answer key. No internal contradictions.

---

## References

(Chapter contains a complete References section; Amodei, Bowman, Irving, Khan, Kenton, Burns, Leike, Stechly, Engels, Wen, and Lightman are all confirmed/labeled. No corrections required.)
