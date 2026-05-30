# Master Fact-Check Report

**Book folder:** validating-output-from-ai-systems
**Date:** 2026-05-30
**Total chapters processed:** 16 content files (frontmatter, introduction, Chapters 1–13, back matter). *(The empty `01-introduction.md` stub was removed at the user's request.)*
**Total files read:** 16 → 16 per-chapter assertion reports in `factchecks/`
**Total assertions flagged:** ~85 (sentences in the six web-verifiable categories)

> The prior drafting-pass report is preserved as `MASTER_REPORT_v1.md` (with `_fc-A.md`, `_fc-B.md`, `_fc-C.md`). This file supersedes it for the structured per-chapter pass.

**Breakdown by content category (approx.):** STAT: 36 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 80 | SPECIALIST: 4 | CURRENT: 18
*(Categories overlap — most flagged sentences are EVIDENCE attributions that are also STAT and/or CURRENT.)*

**Breakdown by assertion type:** BASIC: ~40 | EMPHATIC: 0 | POSITIVE: ~45 | I-LANGUAGE: 0 | COMBINATION: 0

**Field-source adaptation:** A software/AI-validation book, so the biomedical authorities were replaced with arXiv, ACL Anthology, IEEE/ACM, NeurIPS/ICLR/ICML proceedings, USENIX, ACM DL (CSCW/CSUR/Human Factors), provider/vendor reports (OpenAI, Veracode), and court records. A prior drafting-pass fact-check (`_fc-A/B/C`) already live-verified Chapters 03, 04, 06, 08, 09, 10, 11, 13; those were reused, and the not-yet-covered chapters (01, 02, 05, 07, 12, front/back) were verified fresh.

---

## Overall Critical Findings

**Two CONTRADICTED findings (both author-attribution errors) — both now FIXED in prose (2026-05-30); zero OUTDATED; zero COMBINATION.** This is an exceptionally well-sourced and epistemically careful book — it carries its own "[verify]" markers, refuses to print unverifiable figures (it explicitly labels the confabulated "27% more issues" statistic "do not cite as fact"), labels every preprint and future-dated source, and includes "What would change my mind" sections. Many specific figures verified *exactly* against primary sources.

### CONTRADICTED (sorted EVIDENCE first)

**File:** 10-human-validation-and-automation-bias.md
**Assertion type:** BASIC · **Category:** EVIDENCE · **Verdict:** CONTRADICTED → **FIXED**
**Sentence:** "Buçinca, Z., et al. (2025). Cognitive Forcing for Better Decision-Making... Partial explanations reduce overreliance more than full ones."
**Finding:** DOI 10.1145/3710946 is authored by **de Jong, Paananen, Tag & van Berkel** (PACMHCI 9(2), Art. CSCW048), not Buçinca et al., and its finding is "partial > baseline," not "partial > full." **Fixed 2026-05-30:** the in-text refinement and the reference entry now credit de Jong et al. with the "partial > baseline" gloss; inline flag removed. The 2021 Buçinca cognitive-forcing citation is correct and unchanged.

**File:** 11-building-a-validation-pipeline.md
**Assertion type:** BASIC · **Category:** EVIDENCE · **Verdict:** CONTRADICTED → **FIXED**
**Sentence:** "Lucy Bainbridge's 'Ironies of Automation' (1983)..."
**Finding:** The author of "Ironies of Automation" (Automatica, 1983) is **Lisanne** Bainbridge, not "Lucy." Paper, year, and substance are correct; only the first name was wrong. **Fixed 2026-05-30:** prose now reads "Lisanne Bainbridge." (The Chapter 10 reference already used "Bainbridge, L.")

*(No OUTDATED or COMBINATION assertions found.)*

---

## Chapter-by-Chapter Summary

| Chapter File | Assertions Flagged | Critical | Outdated | Contradicted | Unverified | Confirmed |
|---|---|---|---|---|---|---|
| 00-frontmatter.md | 0 | 0 | 0 | 0 | 0 | 0 |
| 00-introduction.md | 0 | 0 | 0 | 0 | 0 | 0 |
| 01-why-validating-ai-output-is-different.md | 7 | 0 | 0 | 0 | 0 (1 not-asserted) | 7 |
| 02-the-deterministic-floor.md | 6 | 0 | 0 | 0 | 0 | 6 |
| 03-validating-generated-code.md | 6 | 0 | 0 | 0 | 0 | 6 |
| 04-validating-factual-claims.md | 7 | 0 | 0 | 0 | 1 (future-dated) | 6 |
| 05-validating-reasoning-chains.md | 7 | 0 | 0 | 0 | 0 | 7 |
| 06-validating-structured-output.md | 6 | 0 | 0 | 0 | 0 | 6 |
| 07-validating-agentic-task-execution.md | 8 | 0 | 0 | 0 | 0 | 8 |
| 08-llm-as-judge-uses-and-failure-modes.md | 8 | 0 | 0 | 0 | 0 | 8 |
| 09-process-reward-models.md | 8 | 0 | 0 | 0 | 0 | 8 |
| 10-human-validation-and-automation-bias.md | 8 | 1 | 0 | 1 | 0 | 7 |
| 11-building-a-validation-pipeline.md | 2 | 1 | 0 | 1 | 0 | 1 |
| 12-scalable-oversight.md | 10 | 0 | 0 | 0 | 0 | 10 |
| 13-capstone-designing-a-validation-strategy.md | 2 | 0 | 0 | 0 | 0 | 2 |
| 99-back-matter.md | 0 | 0 | 0 | 0 | 0 | 0 |
| **TOTAL** | **~85** | **2** | **0** | **2** | **~2** | **~81** |

---

## Recommended Next Steps

The book is in excellent factual shape. Across ~85 verifiable claims spanning the AI-validation literature — Floyd/Hoare, the self-correction results (Huang, Stechly), Chen Self-Debug / Gehring RLEF, PAL and Self-Consistency, PRMs (Lightman, Math-Shepherd, ThinkPRM, OmegaPRM), the LLM-as-judge bias literature (Wang, Zheng, Cohen's kappa, Shi, Panickssery, Verga), agentic evaluation (AgentBench, WebArena, Agent-as-a-Judge, AgentRewardBench, CLEAR), the scalable-oversight canon (Amodei, Bowman, Irving, Khan, Kenton, Burns, Engels/Tegmark), the automation-bias literature (Lyell & Coiera, Skitka, Parasuraman & Manzey, Bainbridge, Reason, Buçinca), Veracode and the AI-code-security studies, and the legal record (Mata v. Avianca) — the overwhelming majority verified **exactly**, including figures the chapters themselves flagged "[verify]."

**The two substantive corrections** are both author-attribution slips, both one-line fixes: (1) Ch.10's partial-explanations reference should credit **de Jong et al.**, not Buçinca, with the gloss "partial > baseline"; (2) Ch.11's "Lucy Bainbridge" should be **Lisanne Bainbridge**. Neither affects an argument.

**Editorial cleanup** (all noted in the per-chapter reports): References sections were **added to ch02 and ch05** during this pass; ch11 and ch13 still lack them (recommended); many resolvable in-text "[verify]" markers can now be removed (the SWE-bench-style figures, Lewis RAG venue, Math-Shepherd/ThinkPRM/OmegaPRM, the Khan/Kenton/Engels figures — all confirmed); the Ch.4 MEGA-RAG reference should be reconciled to author **Xu, S.** (the prose was corrected, the reference still says Xiong); the introduction's chapter map still lists a phantom "Chapter 1: Introduction" (the removed stub) with duplicate-Ch1 numbering; and the back-matter Notes/References/Glossary are unfilled (the glossary terms are just the title words split apart).

**Categories that produced the most flags:** EVIDENCE (named-paper attributions) and STAT (benchmark figures) — exactly where the book invests, and where it proved most reliable. **No GUIDELINE or APPROVAL failures, no OUTDATED claims, and no emphatic-and-unhedged (COMBINATION) overclaims.** Where the field is genuinely unsettled (scalable oversight, agentic step-error correlation, constrained-decoding quality cost, "verification easier than generation" for LLMs), the chapters say so explicitly and pre-register what would change their minds. Overall reliability: **high.**
