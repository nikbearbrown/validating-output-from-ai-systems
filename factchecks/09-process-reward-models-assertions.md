# Assertions Report: 09-process-reward-models.md

**Date:** 2026-05-30
**Source file:** chapters/09-process-reward-models.md
**Assertions flagged:** 8
**Breakdown:** STAT: 4 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 8 | SPECIALIST: 0 | CURRENT: 1

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED.

---

## Full Findings

### EVIDENCE / STAT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Lightman et al. (2023, 'Let's Verify Step by Step')... The result was PRM800K — 800,000 step-level labels — and the headline finding: a process-supervised verifier substantially outperformed an outcome-supervised one... solving 78% of a representative MATH test subset."
**Claim checked:** PRM800K (800K labels); process > outcome; 78% MATH (arXiv:2305.20050).
**Site visited:** arXiv:2305.20050 (confirmed prior pass, factchecks/_fc-B).
**Finding:** Confirmed — Lightman et al. release PRM800K (800K step-level human labels); the process-supervised verifier solves 78% of a representative MATH subset under best-of-N and beats outcome supervision. Accurate.
**Expert review needed:** No
**Suggested reference:** Lightman, H., et al. Let's Verify Step by Step. 2023. arXiv:2305.20050.
**Notes:** Chapter's "[verify venue]" — arXiv ID is the stable reference; later at ICLR 2024.

### STAT / EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Math-Shepherd (Wang et al. 2024)... Mistral-7B went from GSM8K 77.9% → 84.1% and MATH 28.6% → 33.0%."
**Claim checked:** Math-Shepherd automated step labels; Mistral-7B figures (arXiv:2312.08935).
**Site visited:** https://arxiv.org/abs/2312.08935 (re-verified live 2026-05-30).
**Finding:** Confirmed — Math-Shepherd (ACL 2024) constructs process-supervision labels via Monte-Carlo rollouts (no human annotation); process RL improves Mistral-7B 77.9%→84.1% (GSM8K) and 28.6%→33.0% (MATH). Exact.
**Expert review needed:** No
**Suggested reference:** Wang, P., et al. Math-Shepherd. ACL 2024. arXiv:2312.08935.
**Notes:** None.

### STAT / EVIDENCE / CURRENT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "ThinkPRM (Khalifa, Mukhal, et al., 2025)... ThinkPRM-14B was trained on roughly 1,000 synthetic verification CoTs — about 1% of PRM800K's labels — and beat discriminative PRMs... On GPQA-Diamond and LiveCodeBench... ThinkPRM surpassed discriminative verifiers trained on the full PRM800K by 8% and 4.5% respectively."
**Claim checked:** ThinkPRM 1% labels; OOD +8%/+4.5% (arXiv:2504.16828).
**Site visited:** https://arxiv.org/abs/2504.16828 (re-verified live 2026-05-30).
**Finding:** Confirmed — generative PRM verifying via CoT; uses ~1% of PRM800K labels, beats discriminative PRMs and LLM-as-judge on ProcessBench/MATH-500/AIME'24; OOD on GPQA-Diamond and LiveCodeBench surpasses full-PRM800K discriminative verifiers by 8% and 4.5%. Exact.
**Expert review needed:** No
**Suggested reference:** Khalifa, M., Mukhal, M., et al. Process Reward Models That Think (ThinkPRM). 2025. arXiv:2504.16828.
**Notes:** Preprint, labeled.

### EVIDENCE / CURRENT — CONFIRMED (preprint)
**Assertion type:** BASIC
**Sentence:** "Luo, L., et al. (2024). ...(OmegaPRM). arXiv:2406.06592."
**Claim checked:** OmegaPRM (MCTS automated process supervision).
**Site visited:** arXiv:2406.06592 (confirmed prior pass, factchecks/_fc-B).
**Finding:** Confirmed — OmegaPRM (Google DeepMind): divide-and-conquer MCTS, binary-search first error, >1.5M annotations, Gemini Pro MATH 51%→69.4%. Accurate.
**Expert review needed:** No
**Suggested reference:** Luo, L., et al. OmegaPRM. 2024. arXiv:2406.06592.
**Notes:** Chapter's "[verify]" resolves to confirmed.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Marvin Minsky framed the credit-assignment problem in 1961... Arthur Samuel's 1959 checkers program already valued intermediate board positions... Richard Sutton's temporal-difference learning."
**Claim checked:** Minsky 1961 credit assignment; Samuel 1959 checkers; Sutton TD-learning.
**Site visited:** Minsky, "Steps Toward Artificial Intelligence," Proc. IRE 49(1):8–30 (1961); Samuel, IBM J. R&D 3(3):210–229 (1959); Sutton & Barto, RL: An Introduction (2018).
**Finding:** All correct — Minsky's credit-assignment framing (1961), Samuel's value of intermediate positions (1959, the ORM-vs-PRM-in-embryo point is a fair reading), and Sutton's TD-learning as temporal credit assignment. Accurate.
**Expert review needed:** No
**Suggested reference:** Minsky, M. 1961; Samuel, A.L. 1959; Sutton, R.S., & Barto, A.G. 2018.
**Notes:** Chapter's "[verify URL]" markers on Minsky/Samuel are URL-format flags; the citations (volume/issue/pages) are correct.

### SPECIALIST — CONFIRMED (computational)
**Assertion type:** BASIC
**Sentence:** Worked example: "$40 shirt, 25% discount → $30; 25% tax on $30 = $7.50; $30 + 7.50 = 37.50."
**Claim checked:** Arithmetic of the step-level example.
**Site visited:** Direct computation.
**Finding:** Correct — $40 × 0.75 = $30; $30 × 1.25 = $37.50. The worked "wrong step" (adding tax to original $40) and its correction are accurate illustrations of step-level error. Accurate.
**Expert review needed:** No
**Suggested reference:** —
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| "in-domain generative-PRM bias largely unstudied" (convergence note) | CURRENT | BASIC | Prior pass softened this to a forward-looking open-question note, not a citation claim. |

---

## AI-Pass Flags

- The chapter is careful to separate the durable mechanism (process > outcome supervision; PRMs as credit assignment) from model-specific figures, and labels preprints (ThinkPRM, OmegaPRM) distinctly.
- The Minsky/Samuel/Sutton lineage is accurate and the "does not extend to essay quality" caveat (Exercise 9.5) is consistent with the book's ground-truth thesis. No contradictions.

---

## References

(Chapter contains a References section; Lightman, Math-Shepherd, ThinkPRM, OmegaPRM, Minsky, Samuel, and Sutton & Barto are confirmed. The "[verify URL]" markers on Minsky/Samuel are format-only; citations are correct.)
