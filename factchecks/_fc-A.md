# Fact-check batch A — Validating Output from AI Systems
Chapters: 03 (code), 04 (factual claims), 06 (structured output). 10 `[verify]` flags resolved.
Method: cross-checked pantry research files, then web-verified each figure (WebSearch + web_fetch on primary sources). Resolved inline.

| chapter | claim | verdict | resolution / source |
|---|---|---|---|
| 03 | Veracode 2025: AI code security flaws in ~45% of 80 tasks across 100+ models; Java worst ~72%; CWE-80 (XSS) ~86%, CWE-117 (log injection) ~88% | CONFIRMED | Dropped `[verify]`. Veracode 2025 GenAI Code Security Report; BusinessWire 2025-07-30; veracode.com blog. All five figures match. |
| 03 | Aljohani et al. 2025 (IEEE-ISTAS / arXiv:2506.11022): 37.6% increase in critical vulnerabilities after 5 refinement rounds; 400 samples / 40 rounds / 4 strategies | CONFIRMED | Dropped `[verify]`. arXiv:2506.11022 (authors Shukla, Joshi, Syed); accepted IEEE-ISTAS 2025 (IEEE Xplore doc 11269659). Added authors to reference. |
| 03 | "21.1% crypto errors from security prompts" sub-figure attributed to 2506.11022 | UNCONFIRMED — REMOVED | Not present in abstract/HTML/summary. Removed the meta-parenthetical from prose; reference now states it could not be confirmed and is not used. |
| 03 | (misconception box / lab-benchmark) same 37.6% and ~45%/~72%/86%/88% figures | CONFIRMED | Dropped `[verify]` (same sources as above). |
| 03 | aging note `[verify]` on quantitative claims | RESOLVED | Reframed from bare flag to "re-verify against source as model generations turn over." |
| 04 | Mata v. Avianca (S.D.N.Y. 2023): $5,000 sanction for fake ChatGPT precedents | CONFIRMED (pre-marked "verified") | No flag; left as-is. 678 F. Supp. 3d 443; Judge Castel; June 22, 2023. |
| 04 | Citation existence ≤ ~0.475; 50–90% of responses not fully supported | UNVERIFIABLE | Sources are future-dated preprints (arXiv:2602.23452 "CiteAudit", arXiv:2603.07287) the author cannot verify. Kept claim, softened to "indicative, not settled" with honest attribution. Distinction itself cited from arXiv:2412.18004 (2024). |
| 04 | MEGA-RAG >40% hallucination reduction in public health | CONFIRMED + AUTHOR CORRECTED | Dropped `[verify]`. Frontiers Public Health 2025;13:1635381 (PMC12540348; PMID 41132171). Abstract states ">40%". Corrected author Xiong → **Xu, S.** (Xu, Yan, Dai, Wu) in prose and reference. |
| 04 | Ayala & Béchard NAACL 2024 (arXiv:2404.08189): RAG significantly reduces hallucination (ServiceNow) | CONFIRMED | No flag in prose; reference accurate. |
| 04 | Mu-SHROOM SemEval-2025 Task 3 (arXiv:2504.11975): span-labeling, 14 languages, 43 teams, 2,618 submissions | CONFIRMED | No flag in prose; figures match arXiv abstract / ACL Anthology 2025.semeval-1.322. |
| 06 | OpenAI Structured Outputs: ~100% schema adherence with strict:true vs ~5–10% prompt-only/JSON-mode failure | PARTLY CONFIRMED | Dropped `[verify]`. 100% eval, <40% for gpt-4-0613, strict:true, constrained decoding all confirmed on OpenAI announcement page. The "5–10% prompt-only" figure is NOT stated verbatim by OpenAI — re-attributed as a commonly-cited derived estimate, vendor/model-version-specific. |

**Verdict tally:** CONFIRMED 6 · CONFIRMED+corrected 1 (MEGA-RAG author) · PARTLY CONFIRMED 1 (OpenAI 5–10%) · UNCONFIRMED/removed 1 (21.1% crypto) · UNVERIFIABLE/softened 1 (citation-corpus %s).
**Remaining UNVERIFIED:** citation-existence/support magnitudes (future-dated preprints) — kept with honest attribution, not printed as settled.
