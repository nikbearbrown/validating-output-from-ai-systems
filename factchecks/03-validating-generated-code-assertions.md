# Assertions Report: 03-validating-generated-code.md

**Date:** 2026-05-30
**Source file:** chapters/03-validating-generated-code.md
**Assertions flagged:** 6
**Breakdown:** STAT: 4 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 5 | SPECIALIST: 0 | CURRENT: 2

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED (the prior pass already removed the one unconfirmed sub-figure, the "21.1% crypto" number).

---

## Full Findings

### STAT / EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Veracode (2025). GenAI Code Security Report — 80 tasks, 100+ models; ~45% overall flaw rate; Java ~72%; CWE-80 ~86%, CWE-117 ~88%."
**Claim checked:** Veracode 2025 GenAI code-security figures.
**Site visited:** Veracode 2025 GenAI Code Security Report / BusinessWire 2025-07-30 (confirmed prior pass, factchecks/_fc-A).
**Finding:** Confirmed — all five figures (≈45% overall, Java ≈72%, CWE-80 XSS ≈86%, CWE-117 log-injection ≈88%, "larger ≠ safer") match the report. Accurate.
**Expert review needed:** No
**Suggested reference:** Veracode. 2025 GenAI Code Security Report.
**Notes:** Chapter flags "[verify exact figures...]" — now confirmed.

### STAT / EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Aljohani, A., et al. (2025). Security Degradation in Iterative AI Code Generation... 37.6% increase in critical vulnerabilities after five iterations."
**Claim checked:** 37.6% increase; 400 samples / 40 rounds / 4 strategies (arXiv:2506.11022, IEEE-ISTAS 2025).
**Site visited:** arXiv:2506.11022 (confirmed prior pass).
**Finding:** Confirmed — +37.6% critical vulnerabilities after 5 refinement rounds; 400 samples / 40 rounds / 4 strategies; accepted IEEE-ISTAS 2025. Accurate.
**Expert review needed:** No
**Suggested reference:** Aljohani, A., et al. Security Degradation in Iterative AI Code Generation. IEEE-ISTAS 2025. arXiv:2506.11022.
**Notes:** The "21.1% crypto" sub-figure was NOT confirmed and was removed in the prior pass — the chapter's reference note correctly says so.

### STAT / EVIDENCE / CURRENT — CONFIRMED (resolves "[verify author list]")
**Assertion type:** POSITIVE
**Sentence:** "Security Vulnerabilities in AI-Generated Code: A Large-Scale Analysis of Public GitHub Repositories. arXiv:2510.26103 — 7,703 files; 4,241 CWEs; 87.9% clean; Python 16.18–18.50% vs TypeScript 2.50–7.14%."
**Claim checked:** 7,703 files; 4,241 CWEs; 87.9% clean; per-language rates.
**Site visited:** https://arxiv.org/abs/2510.26103 (re-verified live 2026-05-30).
**Finding:** Confirmed — Schreiber & Tippe (2025): 7,703 AI-attributed files (ChatGPT 91.52%, Copilot 7.50%, CodeWhisperer 0.52%, Tabnine 0.46%); CodeQL found 4,241 CWE instances across 77 types; 87.9% of files have no CWE-mapped vulnerability; per-language differences as stated. Accurate.
**Expert review needed:** No
**Suggested reference:** Schreiber, K., & Tippe, B. Security Vulnerabilities in AI-Generated Code: A Large-Scale Analysis of Public GitHub Repositories. 2025. arXiv:2510.26103.
**Notes:** The chapter's "[verify author list]" resolves to Schreiber & Tippe.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "DeMillo, R. A., Lipton, R. J., & Sayward, F. G. (1978). Hints on Test Data Selection... origin of mutation testing." + "Beck, K. (2003). Test-Driven Development." + Hoare 1969.
**Claim checked:** Mutation-testing origin; TDD; Hoare references.
**Site visited:** IEEE Computer 11(4):34–41 (1978); Beck 2003; CACM 12(10) 1969.
**Finding:** All correct — DeMillo/Lipton/Sayward 1978 founded mutation testing; Beck 2003 TDD; Hoare 1969 axiomatic basis. Accurate.
**Expert review needed:** No
**Suggested reference:** DeMillo, R.A., et al. Hints on Test Data Selection. IEEE Computer 11(4), 1978.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Tooling: PIT (Java mutation testing), Stryker (JS/TS), mutmut (Python), CodeQL (SAST)."
**Claim checked:** Named tools and their roles.
**Site visited:** Tool documentation.
**Finding:** Correct — PIT (Java mutation), Stryker (JS/TS mutation), mutmut (Python mutation), CodeQL (SAST). Accurate.
**Expert review needed:** No
**Suggested reference:** PIT / Stryker / mutmut / CodeQL docs.
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| aging note on quantitative claims | CURRENT | BASIC | Prior pass reframed the bare "[verify]" to "re-verify against source as model generations turn over" — an honest aging caveat, not a dubious claim. |

---

## AI-Pass Flags

- The chapter carries resolvable "[verify]" markers (Veracode figures, 37.6%, GitHub-analysis author list) — all now confirmed.
- The prior pass already removed the unconfirmed "21.1% crypto" sub-figure; the reference note documents this honestly. No internal contradictions.

---

## References

(Chapter contains a References section; Veracode, Aljohani, the GitHub-analysis paper, DeMillo, Beck, and Hoare are confirmed. Recommended: resolve the "[verify author list]" to "Schreiber & Tippe.")
