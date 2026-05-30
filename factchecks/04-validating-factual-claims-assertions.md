# Assertions Report: 04-validating-factual-claims.md

**Date:** 2026-05-30
**Source file:** chapters/04-validating-factual-claims.md
**Assertions flagged:** 7
**Breakdown:** STAT: 3 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 6 | SPECIALIST: 0 | CURRENT: 2

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. The one genuinely-unverifiable cluster (citation-corpus percentages) traces to future-dated preprints and is explicitly **not asserted as settled** by the author.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Mata v. Avianca... Judge P. Kevin Castel sanctioned the lawyers $5,000... June 22, 2023; fabricated ChatGPT-generated precedents."
**Claim checked:** Mata v. Avianca sanction.
**Site visited:** 678 F. Supp. 3d 443 (S.D.N.Y. 2023) (confirmed prior pass, factchecks/_fc-A).
**Finding:** Confirmed — $5,000 Rule 11 sanction, Judge Castel, June 22 2023, fabricated AI precedents. Accurate.
**Expert review needed:** No
**Suggested reference:** Mata v. Avianca, Inc., 678 F. Supp. 3d 443 (S.D.N.Y. 2023).
**Notes:** None.

### STAT / EVIDENCE — CONFIRMED (author corrected in prior pass)
**Assertion type:** POSITIVE
**Sentence:** "MEGA-RAG... >40% hallucination reduction vs. baselines... Frontiers in Public Health, 13:1635381."
**Claim checked:** MEGA-RAG >40% reduction; venue.
**Site visited:** Frontiers Public Health 2025;13:1635381 (PMC12540348) (confirmed prior pass).
**Finding:** Confirmed — abstract states ">40%" hallucination reduction. The prior pass corrected the lead author to **Xu, S.** (the reference list shows "Xiong, X." — see note). Accurate figure.
**Expert review needed:** No (verify author spelling)
**Suggested reference:** Xu, S., et al. MEGA-RAG. Frontiers in Public Health 13:1635381, 2025.
**Notes:** **Author-name discrepancy:** the prior pass (factchecks/_fc-A) corrected Xiong → **Xu, S.** in the prose, but the current References entry still reads "Xiong, X., et al." Reconcile to Xu, S.

### STAT / EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Vázquez, R., et al. (2025). SemEval-2025 Task 3: Mu-SHROOM... 14 languages, 38 LLMs, 2,618 submissions / 43 teams."
**Claim checked:** Mu-SHROOM figures (arXiv:2504.11975).
**Site visited:** arXiv:2504.11975 / ACL Anthology 2025.semeval-1.322 (confirmed prior pass).
**Finding:** Confirmed — span-labeling hallucination task, 14 languages, 43 teams, 2,618 submissions. Accurate.
**Expert review needed:** No
**Suggested reference:** Vázquez, R., et al. SemEval-2025 Task 3: Mu-SHROOM. 2025. arXiv:2504.11975.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Ayala, O. M., & Béchard, P. (2024). Reducing Hallucination in Structured Outputs via Retrieval-Augmented Generation. NAACL 2024 (Industry Track)."
**Claim checked:** RAG reduces hallucination (arXiv:2404.08189).
**Site visited:** arXiv:2404.08189 (confirmed prior pass).
**Finding:** Confirmed — ServiceNow; RAG significantly reduces hallucination in structured output. NAACL 2024 Industry. Accurate.
**Expert review needed:** No
**Suggested reference:** Ayala, O.M., & Béchard, P. NAACL 2024 Industry, 228–238. arXiv:2404.08189.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Garfield, E. (1955). Citation Indexes for Science. Science, 122(3159)." + "Bush, V. (1945). As We May Think. The Atlantic." + "Lewis, P., et al. (2020). Retrieval-Augmented Generation... NeurIPS 2020."
**Claim checked:** Garfield 1955, Bush 1945, Lewis RAG 2020 references.
**Site visited:** Science 122(3159):108–111; The Atlantic 1945; NeurIPS 2020 (arXiv:2005.11401).
**Finding:** All correct — Garfield's citation-index paper (1955), Bush's "As We May Think" (1945), Lewis et al.'s canonical RAG paper (NeurIPS 2020). Accurate.
**Expert review needed:** No
**Suggested reference:** Lewis, P., et al. Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS 2020. arXiv:2005.11401.
**Notes:** Chapter's "[verify venue/year]" on Lewis resolves to NeurIPS 2020.

### EVIDENCE — CONFIRMED (existence) / figures UNVERIFIABLE
**Assertion type:** BASIC
**Sentence:** "'Correctness is not Faithfulness' (2024). arXiv:2412.18004. Formalizes the support-vs-correctness distinction." + the citation-existence/support magnitudes.
**Claim checked:** The support-vs-correctness distinction (2412.18004) and the corpus percentages.
**Site visited:** arXiv:2412.18004 (confirmed prior pass); the percentage sources are future-dated preprints.
**Finding:** The support-vs-correctness *distinction* is established (arXiv:2412.18004, 2024). The specific magnitudes ("existence ≤ ~0.475; 50–90% not fully supported") trace to future-dated preprints (arXiv:2602.23452, arXiv:2603.07287) the author cannot verify; the chapter explicitly keeps them "indicative, not settled."
**Expert review needed:** No (already honestly handled)
**Suggested reference:** "Correctness is not Faithfulness." 2024. arXiv:2412.18004.
**Notes:** Chapter's "[verify title/authors]" on 2412.18004 remains open; the distinction itself is sound.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| "existence ≤ ~0.475; 50–90% of responses not fully supported" | STAT | POSITIVE | Sourced from future-dated preprints (arXiv:2602.23452, arXiv:2603.07287); the author keeps them "indicative, not settled." Not asserted as fact. |

---

## AI-Pass Flags

- **Author-name discrepancy (MEGA-RAG):** prose says Xu, S. (corrected); References still say Xiong, X. Reconcile.
- Resolvable "[verify]" markers (Lewis venue → NeurIPS 2020; MEGA-RAG >40% phrasing → confirmed).
- The chapter cleanly separates the established support-vs-correctness *distinction* from the unverifiable *magnitudes*. No internal contradictions.

---

## References

(Chapter contains a References section; Mata v. Avianca, MEGA-RAG, Mu-SHROOM, Ayala & Béchard, Garfield, Bush, Lewis, and 2412.18004 are confirmed. Fix the MEGA-RAG author (Xu, S.) and keep the corpus percentages non-asserted.)
