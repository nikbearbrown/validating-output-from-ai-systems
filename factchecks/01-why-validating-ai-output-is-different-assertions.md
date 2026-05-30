# Assertions Report: 01-why-validating-ai-output-is-different.md

**Date:** 2026-05-30
**Source file:** chapters/01-why-validating-ai-output-is-different.md
**Assertions flagged:** 7
**Breakdown:** STAT: 1 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 6 | SPECIALIST: 1 | CURRENT: 0

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED; the one dubious figure (Wang et al. "27%") is explicitly **not asserted** — the author flags it inline and attributes the direction to Lyell & Coiera instead.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Floyd (1967) and Hoare (1969) made the verification of a program a separate, formal activity from its writing, by attaching pre- and post-conditions..."
**Claim checked:** Floyd 1967 / Hoare 1969 program-verification origins.
**Site visited:** Floyd, "Assigning Meanings to Programs" (1967); Hoare, "An Axiomatic Basis for Computer Programming," CACM 12(10):576–580 (1969), DOI 10.1145/363235.363259.
**Finding:** Correct — both canonical citations; Floyd's meanings/Hoare's axiomatic basis (pre/post-conditions) founded formal program verification. Accurate.
**Expert review needed:** No
**Suggested reference:** Hoare, C.A.R. An Axiomatic Basis for Computer Programming. CACM 12(10), 1969.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Stechly, Valmeekam & Kambhampati (2024) tested LLM self-verification on reasoning and planning tasks... and found it unreliable..."
**Claim checked:** Self-verification limitations (arXiv:2402.08115).
**Site visited:** https://arxiv.org/abs/2402.08115 (re-verified live 2026-05-30).
**Finding:** Confirmed — "On the Self-Verification Limitations of LLMs on Reasoning and Planning Tasks" (GPT-4 on Game of 24, Graph Coloring, STRIPS planning); challenges the belief that LLMs reliably self-critique. Accurate.
**Expert review needed:** No
**Suggested reference:** Stechly, K., Valmeekam, K., & Kambhampati, S. On the Self-Verification Limitations of LLMs. 2024. arXiv:2402.08115.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Huang et al. (2024) tested intrinsic self-correction... and found that models do not dependably improve and sometimes get worse after 'correcting.'"
**Claim checked:** Intrinsic self-correction degrades (arXiv:2310.01798).
**Site visited:** arXiv:2310.01798 (confirmed in companion-book passes; ICLR 2024).
**Finding:** Accurate. Confirmed.
**Expert review needed:** No
**Suggested reference:** Huang, J., et al. Large Language Models Cannot Self-Correct Reasoning Yet. ICLR 2024. arXiv:2310.01798.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "The finding that matters most here comes from Lyell & Coiera (2017), a systematic review establishing that automation bias rises with verification complexity and cognitive load."
**Claim checked:** Lyell & Coiera 2017 automation-bias review (JAMIA 24(2):423–431).
**Site visited:** DOI 10.1093/jamia/ocw105 (confirmed prior pass, factchecks/_fc-C).
**Finding:** Confirmed — "Automation bias and verification complexity: a systematic review," JAMIA 2017; automation bias increases with verification difficulty / cognitive load. Accurate.
**Expert review needed:** No
**Suggested reference:** Lyell, D., & Coiera, E. JAMIA 24(2):423–431, 2017. DOI 10.1093/jamia/ocw105.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Skitka, Mosier and colleagues documented it in trained aircrews... making both error types when an imperfect but reliable automated aid was present."
**Claim checked:** Skitka/Mosier automation-bias aviation studies (Skitka, Mosier & Burdick 1999, IJHCS 51(5):991–1006).
**Site visited:** DOI 10.1006/ijhc.1999.0252.
**Finding:** Correct — Skitka, Mosier & Burdick (1999) is the canonical automation-bias (omission/commission errors) study. Accurate; the chapter's "[verify exact pages/issue]" marker resolves to 51(5):991–1006.
**Expert review needed:** No
**Suggested reference:** Skitka, L.J., Mosier, K.L., & Burdick, M. Does automation bias decision-making? IJHCS 51(5):991–1006, 1999.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "sanctioned filings of fabricated AI-generated citations are now a documented genre in U.S. courts."
**Claim checked:** Sanctioned fabricated-AI-citation filings.
**Site visited:** Mata v. Avianca (S.D.N.Y. 2023) and successors (covered in Ch.4 pass).
**Finding:** Confirmed — a documented and growing genre (Mata v. Avianca, 2023, and many since). Accurate.
**Expert review needed:** No
**Suggested reference:** Mata v. Avianca, Inc., 678 F. Supp. 3d 443 (S.D.N.Y. 2023).
**Notes:** Developed in Chapter 4.

### SPECIALIST — CONFIRMED (computational)
**Assertion type:** POSITIVE
**Sentence:** "P(success) ≈ 0.9^10 ≈ 0.35 ... Push to twenty steps and 0.9^20 ≈ 0.12."
**Claim checked:** Compounding-error arithmetic.
**Site visited:** Direct computation.
**Finding:** 0.9^10 = 0.3487 (≈0.35); 0.9^20 = 0.1216 (≈0.12). Correct. The chapter explicitly labels the independence assumption "illustrative, not a measured law." Accurate.
**Expert review needed:** No
**Suggested reference:** —
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| "reviewers checking AI-generated output miss substantially more issues than independent reviewers — often cited as roughly 27% more, from a Wang et al. (2024) study." | STAT | POSITIVE | **Explicitly not asserted.** The author writes "I have not been able to confirm that specific study, number, or effect size" and carries the direction via Lyell & Coiera instead. Prior pass (factchecks/_fc-C) classified the 27% figure as confabulated and confirmed it is not asserted as fact anywhere. No inline flag needed — already handled. |

---

## AI-Pass Flags

- The opening date-bug PR scenario is a labeled illustrative scenario.
- The chapter is a model of responsible sourcing: it openly refuses to print the unverifiable "27%" figure, labels the compounding-error model "illustrative," and pre-registers what would change its mind. No internal contradictions.
- The References list retains author `[verify exact pages]` markers on Skitka 1999 and Mosier & Skitka 1996 — Skitka 1999 = IJHCS 51(5):991–1006 (confirmed); the Wang 2024 entry is correctly self-flagged as "do NOT state as established."

---

## References

(Chapter contains an accurate References section; Floyd, Hoare, Stechly, Huang, Lyell & Coiera, and Skitka are confirmed. No corrections required; the Wang et al. entry should remain non-asserted or be removed.)
