# Assertions Report: 02-the-deterministic-floor.md

**Date:** 2026-05-30
**Source file:** chapters/02-the-deterministic-floor.md
**Assertions flagged:** 6
**Breakdown:** STAT: 1 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 4 | SPECIALIST: 1 | CURRENT: 1

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED; vendor-illustrative SAST figures and the constrained-decoding quality-cost tradeoff are explicitly flagged in-text as unsettled.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Huang et al. (2024) showed that intrinsic self-correction — re-examination with no external signal — does not dependably improve answers and can degrade them."
**Claim checked:** Intrinsic self-correction (arXiv:2310.01798).
**Site visited:** arXiv:2310.01798 (confirmed Ch.1).
**Finding:** Accurate. Confirmed.
**Expert review needed:** No
**Suggested reference:** Huang, J., et al. ICLR 2024. arXiv:2310.01798.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Chen et al. (2024), 'Teaching Large Language Models to Self-Debug,' showed... when you feed a model execution feedback... code-generation accuracy improves materially."
**Claim checked:** Self-Debug + execution feedback (arXiv:2304.05128).
**Site visited:** arXiv:2304.05128 (confirmed in companion-book pass; ICLR 2024).
**Finding:** Correct — Chen, Lin, Schärli & Zhou; Self-Debug improves most with execution feedback. Accurate.
**Expert review needed:** No
**Suggested reference:** Chen, X., et al. Teaching Large Language Models to Self-Debug. ICLR 2024. arXiv:2304.05128.
**Notes:** Paper is 2023 (arXiv) / ICLR 2024; the chapter's "(2024)" tracks the conference year.

### EVIDENCE / CURRENT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Gehring et al. (2024, RLEF) push the same principle into the training loop: grounding generation in execution results drives reliability at scale."
**Claim checked:** RLEF (arXiv:2410.02089).
**Site visited:** https://arxiv.org/abs/2410.02089 (re-verified live 2026-05-30).
**Finding:** Confirmed — "RLEF: Grounding Code LLMs in Execution Feedback with Reinforcement Learning" (Gehring et al., Meta 2024); PPO-trained agents using execution results, SOTA on CodeContests/HumanEval+/MBPP+. Accurate.
**Expert review needed:** No
**Suggested reference:** Gehring, J., et al. RLEF. 2024. arXiv:2410.02089.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Niklaus Wirth's argument that strict static typing is a feature... Frances Allen's program-analysis lineage — reasoning about a program's properties from its source without executing it."
**Claim checked:** Wirth (strong static typing) and Allen (program analysis) attributions.
**Site visited:** History of programming languages / compiler theory (Wirth; Allen, Turing Award 2006).
**Finding:** Both correct — Wirth (Pascal/Modula designer) championed strong static typing; Frances Allen pioneered program analysis and optimization. Accurate as lineage attributions.
**Expert review needed:** No
**Suggested reference:** Wirth, N. (Pascal/Modula); Allen, F.E. (program analysis/optimization).
**Notes:** Interpretive lineage; no specific paper cited (acceptable for the framing).

### SPECIALIST / CURRENT — CONFIRMED (mechanism)
**Assertion type:** POSITIVE
**Sentence:** "Constrained decoding moves the check into generation. Compile the schema into a finite-state machine, and at each decoding step mask out any token that would make the partial output unable to complete into a valid document... structurally-invalid output becomes unrepresentable."
**Claim checked:** Constrained-decoding / FSM token-masking mechanism; structured-output mode adoption.
**Site visited:** OpenAI Structured Outputs (confirmed Ch.6 prior pass); Willard & Louf guided-generation (arXiv:2307.09702).
**Finding:** Confirmed — grammar/FSM-constrained decoding masks invalid tokens at each step, making invalid shape unrepresentable; provider structured-output modes use this. Accurate. The chapter flags the quality-cost tradeoff as contested.
**Expert review needed:** No
**Suggested reference:** OpenAI, Structured Outputs (2024); Willard & Louf, Efficient Guided Generation, arXiv:2307.09702.
**Notes:** None.

### STAT — CONFIRMED (vendor-illustrative, labeled / not asserted)
**Assertion type:** POSITIVE (hedged)
**Sentence:** "a one-million-line codebase might surface tens of thousands of findings, with a few percent being false positives — but these are vendor-illustrative figures, not peer-reviewed constants."
**Claim checked:** SAST scale/noise figures.
**Site visited:** (Not applicable — the chapter explicitly labels these vendor-illustrative and flags "[verify]".)
**Finding:** The figures are presented only "to make the tradeoff concrete," explicitly disclaimed as not measured constants. Not asserted as fact; no verification burden.
**Expert review needed:** No
**Suggested reference:** Vendor-illustrative (SAST tooling docs).
**Notes:** Appropriately quarantined.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| "whether forcing the FSM degrades content quality while guaranteeing shape... The evidence is mixed and model-dependent." | CURRENT | BASIC (hedged) | The author flags this "[verify — contested and model-specific]"; it is an honest open-question note, not an asserted finding. |
| SAST "one-million-line / tens of thousands / few percent" figures | STAT | POSITIVE | Explicitly vendor-illustrative, not asserted (see above). |

---

## AI-Pass Flags

- The "would you ship code that doesn't compile?" framing and the off-by-one example are pedagogical, consistent with Chapter 1.
- The chapter is careful to distinguish the floor's *strength* (exact, fluency-immune) from its *ceiling* (form ≠ intent) and labels every soft figure. No internal contradictions.

---

## References

(Chapter cites Huang, Chen Self-Debug, Gehring RLEF, Wirth, Allen, and constrained decoding — all confirmed. If the chapter lacks a formal References section, consider adding one with these entries; the inline attributions are accurate.)
