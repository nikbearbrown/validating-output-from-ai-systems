# Assertions Report: 13-capstone-designing-a-validation-strategy.md

**Date:** 2026-05-30
**Source file:** chapters/13-capstone-designing-a-validation-strategy.md
**Assertions flagged:** 2
**Breakdown:** STAT: 0 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 2 | SPECIALIST: 0 | CURRENT: 0

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Genichi Taguchi's robust-design insight is the lens on this whole procedure: quality is the loss imparted by variation you couldn't remove, and you design a system to be insensitive to the noise you can't eliminate."
**Claim checked:** Taguchi robust design / quality-loss function.
**Site visited:** Quality-engineering literature (Taguchi methods; the quadratic quality-loss function and robust/parameter design).
**Finding:** Correct — Genichi Taguchi's robust design defines quality as loss to society from variation and designs systems insensitive to uncontrollable noise; the "tolerance dial set by stakes" gloss is an accurate use. Accurate.
**Expert review needed:** No
**Suggested reference:** Taguchi, G. Introduction to Quality Engineering / Taguchi methods (robust/parameter design, quality-loss function).
**Notes:** None.

### EVIDENCE — CONFIRMED (back-references)
**Assertion type:** BASIC
**Sentence:** The seven-step decision flow composes Chapters 2–12 and the worked RAG example references claim-support/entailment ("Ragas-style metrics"), the deterministic floor, trajectory checkpoints, and the residual-risk box, with figures (CLEAR 60%→25%, Veracode, +37.6% iterative security degradation) carried from their home chapters.
**Claim checked:** Back-referenced figures and named methods (Ragas; the seven-step composition).
**Site visited:** Resolved in home chapters (Ch.2–12) and prior pass (factchecks/_fc-C).
**Finding:** All back-referenced figures are confirmed in their home chapters; "Ragas-style" RAG-evaluation metrics is a real, correctly-used reference (the Ragas framework). The chapter introduces no new unverified figures and labels its worked system "a labeled design exercise — not a description of any shipped product." Accurate.
**Expert review needed:** No
**Suggested reference:** (see Chapters 2–12 references; Ragas RAG-evaluation framework.)
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| — | — | — | The capstone is an integrative template + labeled worked example; it introduces no new web-verifiable claims beyond Taguchi and back-references. |

---

## AI-Pass Flags

- The worked RAG support-assistant example is explicitly labeled "a labeled design exercise — not a description of any shipped product."
- The chapter's insistence on a residual-risk / "cannot-certify" statement is consistent with Chapter 12's "unsolved" verdict. No internal contradictions.
- **No References section.** Consider adding one (Taguchi; the Ragas framework; pointers to the figures' home chapters) for standalone readability.

---

## References

(Chapter has no formal References section. Recommended: Taguchi (quality engineering / robust design); Ragas (RAG-evaluation framework); and pointers to Chapters 2–12 for the composed figures.)
