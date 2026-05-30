# Assertions Report: 06-validating-structured-output.md

**Date:** 2026-05-30
**Source file:** chapters/06-validating-structured-output.md
**Assertions flagged:** 6
**Breakdown:** STAT: 1 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 5 | SPECIALIST: 1 | CURRENT: 2

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. The OpenAI structured-output figures are vendor-reported and model-version-specific, which the chapter labels; the constrained-decoding quality-loss magnitudes are flagged "[verify]."

---

## Full Findings

### EVIDENCE / CURRENT — CONFIRMED (vendor figures labeled)
**Assertion type:** POSITIVE
**Sentence:** "OpenAI (2024). Introducing Structured Outputs in the API — vendor-reported 100% schema eval, <0.1% vs 5–10% JSON-mode failure for gpt-4o-2024-08-06."
**Claim checked:** OpenAI Structured Outputs schema-adherence figures.
**Site visited:** OpenAI Structured Outputs announcement (confirmed prior pass, factchecks/_fc-A).
**Finding:** Partly confirmed — the 100% eval and the near-perfect strict:true adherence (vs. <40% for gpt-4-0613) are on OpenAI's page; the "5–10% prompt-only" figure is a commonly-cited derived estimate, not stated verbatim by OpenAI, and the chapter labels these vendor/model-version-specific. Accurate as framed.
**Expert review needed:** No
**Suggested reference:** OpenAI. Introducing Structured Outputs in the API. 2024.
**Notes:** Prior pass re-attributed the "5–10%" as a derived estimate.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Willard, B. T., & Louf, R. (2023). Efficient Guided Generation for Large Language Models. arXiv:2307.09702 — the Outlines library; finite-state-machine constrained decoding."
**Claim checked:** Outlines / FSM constrained decoding.
**Site visited:** arXiv:2307.09702 (confirmed in companion-book pass).
**Finding:** Correct — Willard & Louf's guided-generation paper (Outlines), FSM token masking. Accurate.
**Expert review needed:** No
**Suggested reference:** Willard, B.T., & Louf, R. Efficient Guided Generation for LLMs. 2023. arXiv:2307.09702.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Tam, Z. R., et al. (2024). Let Me Speak Freely? A Study on the Impact of Format Restrictions on the Performance of Large Language Models. arXiv:2408.02442."
**Claim checked:** Format-restriction reasoning cost.
**Site visited:** arXiv:2408.02442 (confirmed in companion-book pass; EMNLP 2024 Industry).
**Finding:** Correct — format restriction can degrade reasoning vs. free-form. Accurate.
**Expert review needed:** No
**Suggested reference:** Tam, Z.R., et al. Let Me Speak Freely? EMNLP 2024 Industry. arXiv:2408.02442.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Liskov, B., & Wing, J. (1994). A behavioral notion of subtyping. ACM TOPLAS 16(6)." + "Backus, J. W. (1959)... Backus–Naur Form."
**Claim checked:** Liskov–Wing subtyping; Backus BNF.
**Site visited:** ACM TOPLAS 16(6), 1994; Backus 1959 (ICIP).
**Finding:** Both correct — the Liskov substitution / behavioral-subtyping paper (1994) and Backus's 1959 paper introducing what became BNF. Accurate as the "schema-as-contract" and grammar lineage.
**Expert review needed:** No
**Suggested reference:** Liskov, B., & Wing, J. ACM TOPLAS 16(6), 1994; Backus, J.W. 1959.
**Notes:** None.

### EVIDENCE / SPECIALIST — CONFIRMED (tools)
**Assertion type:** BASIC
**Sentence:** "Pydantic (Python) and Zod (TypeScript) compile such contracts into runtime validators." + JSON Schema.
**Claim checked:** Pydantic/Zod/JSON Schema roles.
**Site visited:** docs.pydantic.dev; zod.dev; json-schema.org.
**Finding:** Correct — Pydantic and Zod compile schemas into runtime validators; JSON Schema is the deterministic contract. Accurate.
**Expert review needed:** No
**Suggested reference:** Pydantic / Zod / JSON Schema docs.
**Notes:** None.

### CURRENT — CONFIRMED (existence; magnitudes flagged)
**Assertion type:** BASIC
**Sentence:** "Constrained-decoding distortion: CRANE (arXiv:2502.09061) and grammar-aligned decoding (arXiv:2405.21047)."
**Claim checked:** The two constrained-decoding-quality papers.
**Site visited:** arXiv:2502.09061, arXiv:2405.21047.
**Finding:** Both are real papers on the constrained-decoding quality/expressivity tradeoff; the chapter flags "[verify exact quality-loss magnitudes per paper before quoting]." Existence and topic confirmed; magnitudes appropriately quarantined.
**Expert review needed:** No
**Suggested reference:** CRANE (arXiv:2502.09061); grammar-aligned decoding (arXiv:2405.21047).
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| "5–10% JSON-mode failure" | STAT | POSITIVE | Not stated verbatim by OpenAI; prior pass re-attributed as a derived, model-version-specific estimate. The chapter labels vendor figures as such. |
| CRANE / grammar-aligned quality-loss magnitudes | CURRENT | BASIC | Author flags "[verify exact magnitudes]"; the papers exist and are on-topic. |

---

## AI-Pass Flags

- The chapter cleanly distinguishes a "mathematical guarantee of shape" (constrained decoding) from a statistical one (prompt-only), consistent with Chapter 2.
- Vendor figures and contested quality-cost magnitudes are labeled. No internal contradictions.

---

## References

(Chapter contains a References section; OpenAI Structured Outputs, Willard & Louf, Tam et al., Liskov & Wing, Backus, the validator tools, and the constrained-decoding-quality papers are confirmed/labeled. No corrections required.)
