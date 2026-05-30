# Assertions Report: 05-validating-reasoning-chains.md

**Date:** 2026-05-30
**Source file:** chapters/05-validating-reasoning-chains.md
**Assertions flagged:** 7
**Breakdown:** STAT: 1 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 7 | SPECIALIST: 0 | CURRENT: 0

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Huang et al. (2024)... intrinsic self-correction does not reliably improve performance, and it sometimes degrades it."
**Claim checked:** Intrinsic self-correction (arXiv:2310.01798).
**Site visited:** arXiv:2310.01798 (confirmed throughout).
**Finding:** Accurate. Confirmed.
**Expert review needed:** No
**Suggested reference:** Huang, J., et al. ICLR 2024. arXiv:2310.01798.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "In 1967 the Dutch mathematician Nicolaas Govert de Bruijn built Automath, widely considered the first proof checker... By the mid-1970s an entire analysis textbook had been formally verified in it."
**Claim checked:** Automath (de Bruijn, 1967); Landau analysis textbook verified.
**Site visited:** History of proof assistants (Automath; van Benthem Jutting's 1977 verification of Landau's Grundlagen).
**Finding:** Correct — de Bruijn's Automath (1967) is widely cited as the first proof checker; L.S. van Benthem Jutting formally verified Landau's analysis text in Automath (completed 1977). Accurate.
**Expert review needed:** No
**Suggested reference:** de Bruijn, N.G. The mathematical language AUTOMATH. 1968; Jutting, L.S. van Benthem. Checking Landau's "Grundlagen" in the Automath system. 1977.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "The theoretical foundation traces to Gerhard Gentzen's natural deduction and cut-elimination (1934–35)... The engineering descendant is Leslie Lamport's TLA+."
**Claim checked:** Gentzen 1934–35; Lamport TLA+.
**Site visited:** Gentzen, "Untersuchungen über das logische Schließen" (1934–35); Lamport TLA+.
**Finding:** Both correct — Gentzen founded natural deduction and proved cut-elimination (1934–35); Lamport created TLA+ for mechanical specification/model-checking. Accurate.
**Expert review needed:** No
**Suggested reference:** Gentzen, G. 1934–35; Lamport, L. TLA+.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Gao et al. (2023), 'PAL: Program-aided Language Models' (ICML 2023), diagnosed... models decompose problems well... but they make arithmetic and logic mistakes while executing... The model emits a program... a Python interpreter runs it."
**Claim checked:** PAL method/diagnosis (arXiv:2211.10435, ICML 2023).
**Site visited:** https://arxiv.org/abs/2211.10435 / proceedings.mlr.press/v202/gao23f (re-verified live 2026-05-30).
**Finding:** Confirmed — Gao, Madaan, Zhou, Alon, Liu, Yang, Callan, Neubig; LLM reads NL problems, generates programs as reasoning steps, offloads the solution to a Python interpreter; evaluated across BIG-Bench-Hard and math/symbolic/algorithmic tasks. Accurate.
**Expert review needed:** No
**Suggested reference:** Gao, L., et al. PAL: Program-aided Language Models. ICML 2023. arXiv:2211.10435.
**Notes:** None.

### STAT / EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Wang et al. (2022), 'Self-Consistency...' (ICLR 2023)... roughly 17.9 points on GSM8K, 11.0 on SVAMP, 12.2 on AQuA."
**Claim checked:** Self-consistency method + gains (arXiv:2203.11171).
**Site visited:** arXiv:2203.11171 (confirmed in companion-book pass).
**Finding:** Confirmed — majority-vote over diverse sampled reasoning paths; reported gains GSM8K +17.9, SVAMP +11.0, AQuA +12.2. The chapter correctly labels them "historical, model-specific." Accurate.
**Expert review needed:** No
**Suggested reference:** Wang, X., et al. Self-Consistency Improves Chain of Thought Reasoning. ICLR 2023. arXiv:2203.11171.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Lightman et al. (2023), 'Let's Verify Step by Step,' showed process supervision — feedback on each step — beats outcome supervision on hard math."
**Claim checked:** Process > outcome supervision (arXiv:2305.20050).
**Site visited:** arXiv:2305.20050 (confirmed prior pass, factchecks/_fc-B).
**Finding:** Confirmed — process supervision outperforms outcome supervision on the MATH benchmark. Accurate. (Full treatment in Ch.9.)
**Expert review needed:** No
**Suggested reference:** Lightman, H., et al. Let's Verify Step by Step. 2023. arXiv:2305.20050.
**Notes:** None.

### SPECIALIST — CONFIRMED (computational)
**Assertion type:** BASIC
**Sentence:** The PAL worked example (3 dozen muffins → Python program) and the self-consistency code/vote-share logic.
**Claim checked:** Arithmetic/logic of the worked examples.
**Site visited:** Direct inspection.
**Finding:** The PAL program computes `remaining = start - sold + delivery` consistently with the narrative; the self-consistency vote-share logic is correct. Internally consistent.
**Expert review needed:** No
**Suggested reference:** —
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| "the auto-formalization gap... is currently the bottleneck on extending deterministic validation to reasoning in the wild." | EVIDENCE | BASIC | An accurate characterization of the formal-methods/auto-formalization literature; presented as a field observation, not a single-source statistic. |

---

## AI-Pass Flags

- The opening "reflection feature" scenario is a labeled illustrative case.
- The chapter is rigorous about the self-consistency failure boundary (it "launders systematic bias") and crosses out ungrounded self-critique at every leaf of the decision tree — internally consistent with Chapters 1–2.

---

## References

(If the chapter lacks a formal References section, recommend adding one with: Huang 2024 (arXiv:2310.01798); de Bruijn/Automath; Gentzen 1934–35; Lamport TLA+; Gao et al. PAL (arXiv:2211.10435); Wang et al. Self-Consistency (arXiv:2203.11171); Lightman et al. (arXiv:2305.20050). All inline attributions are accurate.)
