# Master Fact-Check Report — Validating Output from AI Systems

**Date:** 2026-05-29  ·  **Author:** Nik Bear Brown
**Scope:** all `[verify]` flags placed during drafting, resolved against the pantry research files and live web sources.

Verdicts: CONFIRMED (source verified) · CORRECTED (figure/attribution fixed inline) · PREPRINT (real but not yet peer-reviewed; labeled in text) · UNVERIFIABLE (no primary source; attributed honestly, never asserted as fact).

---

## Batch: A

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

---

## Batch: B

# Fact-check B — verdicts

Chapters: 08-llm-as-judge-uses-and-failure-modes, 09-process-reward-models
Date: 2026-05-29

| chapter | claim | verdict | resolution/source |
|---|---|---|---|
| 08 | §8.1 intro: arXiv:2604.23178 reports position bias negligible (≤0.04), style bias dominant (0.76–0.92) | CONFIRMED (preprint) | Indexed; single-author preprint (Soumik). Dropped [verify], labeled [preprint, not yet peer-reviewed]. arXiv:2604.23178 |
| 08 | §8.4 prose: same 2604.23178 position-down/style-up inversion | CONFIRMED (preprint) | Same source; relabeled "fresh single-author preprint, not yet peer-reviewed." arXiv:2604.23178 |
| 08 | §8.4 table — Position row: "negligible on frontier models per 2604.23178" | CONFIRMED (preprint) | Added ≤0.04 figure; [preprint] label. arXiv:2604.23178 |
| 08 | §8.4 table — Style row: "Dominant per 2604.23178" | CONFIRMED (preprint) | Added 0.76–0.92; [preprint] label. arXiv:2604.23178 |
| 08 | §8.4 self-preference: arXiv:2504.03846 contests bias-vs-calibration framing | CONFIRMED | Chen et al. 2025; verifiable-benchmark study — self-preference mostly legitimate, harmful persists when evaluator errs as generator. Attributed + [preprint]. arXiv:2504.03846 |
| 08 | "Still puzzling": self-preference bias-or-signal (2504.03846) | CONFIRMED | Same; attributed with finding. arXiv:2504.03846 |
| 08 | "Still puzzling": preference leakage as distinct channel (2502.01534) | CONFIRMED | Li et al., "Preference Leakage," ICLR 2026 — judges favor related student models. Cited fully, dropped [verify]. arXiv:2502.01534 |
| 08 | "What would change my mind": position-down/style-up rests on 2604.23178 | CONFIRMED (preprint) | Relabeled single-author preprint, not yet peer-reviewed. arXiv:2604.23178 |
| 08 | References: 2604.23178 entry | CONFIRMED (preprint) | Added author (Soumik), study scope; [preprint] label. Added 2504.03846 and 2502.01534 reference entries. |
| 09 | §9.2: Lightman "Let's Verify Step by Step" posted 2023, arXiv:2305.20050 | CONFIRMED | Date 2023 correct; venue-verify flag dropped (arXiv ID is stable reference). arXiv:2305.20050 |
| 09 | §9.3: OmegaPRM (2406.06592) extends MC labeling via MCTS | CONFIRMED (upgraded) | Was "not full-text confirmed"; now fully verified — Luo et al. 2024, Google DeepMind, divide-and-conquer MCTS, binary-search first error, >1.5M annotations, Gemini Pro 51%→69.4% MATH. Added details, [preprint]. arXiv:2406.06592 |
| 09 | §9.6 convergence: in-domain generative-PRM bias largely unstudied | UNVERIFIABLE (open question) | Softened to a forward-looking statement; bare [verify] removed (claim is an open-research note, not a citation). |
| 09 | References: OmegaPRM entry (2406.06592) | CONFIRMED (preprint) | Filled in DeepMind, MCTS, 1.5M annotations, MATH 51%→69.4%; [preprint] label; dropped [verify]. arXiv:2406.06592 |

## Tally
- Chapter 08: 9 [verify] flags resolved — all CONFIRMED (5 as preprint-labeled: 2604.23178 ×5 contexts treated, 2504.03846 ×2, 2502.01534).
- Chapter 09: 4 [verify] flags resolved — 3 CONFIRMED (1 upgraded from unconfirmed), 1 UNVERIFIABLE-softened (open-question note).
- Remaining UNVERIFIED: none.
- Out of scope (left untouched): ch09 References `[verify exact URL]` on Minsky (1961) and Samuel (1959) — URL-format flags, not content claims.

## Notes
- All four future/recent arXiv IDs (2604.23178, 2504.03846, 2502.01534, 2406.06592) resolved to live abstract pages via WebSearch and match the research-file descriptions.
- No fabricated sources. No curl/python used.

---

## Batch: C

# Fact-check C — Chapters 10, 11, 13

| chapter | claim | verdict | resolution/source |
|---|---|---|---|
| 10 | Buçinca et al. 2025 partial-explanations follow-on (DOI 10.1145/3710946) | CONFIRMED (misattributed) | Paper real but authored by **de Jong, Paananen, Tag & van Berkel**, *PACMHCI* 9(2), Art. CSCW048, Apr 2025. Fixed attribution; corrected gloss (partial > baseline, not > full). https://dl.acm.org/doi/10.1145/3710946 |
| 10 | Adversarial review framing has direct RCT evidence | UNVERIFIABLE (no such study) | No study isolates adversarial framing as an intervention; resolved to explicit "no direct trial; rests on shared engagement mechanism" honesty note. Not asserted as evidence. |
| 10 | Alert-fatigue analogue, ACM Computing Surveys DOI 10.1145/3723158 | CONFIRMED | Tariq, Baruwal Chhetri, Nepal & Paris, "Alert Fatigue in Security Operations Centres: Research Challenges and Opportunities," *ACM Comput. Surv.* 57(9), Art. 224 (2025). Vendor FP-rate figures relabeled "practitioner signal, not peer-reviewed." |
| 10 (ref) | de Jong/Buçinca 2025 partial-explanations ref entry | CONFIRMED | Same as above; ref corrected to de Jong et al., vol 9(2), Art. CSCW048. |
| 10 (ref) | Parasuraman & Manzey 2010, Human Factors 52(3):381–410, DOI 10.1177/0018720810376055 | CONFIRMED | SAGE/Human Factors record confirms vol 52(3), pp. 381–410, June 2010. Flag dropped. |
| 10 (ref) | "Alert Fatigue in SOCs," ACM CSUR DOI 10.1145/3723158 | CONFIRMED | Full title + authors + 57(9) Art. 224 added. |
| 10 (ref) | CSET "AI Safety and Automation Bias" issue brief + URL | CONFIRMED | CSET Georgetown, Nov 2024, Kahn, Probasco & Kinoshita. URL valid. Flag dropped; authors added. |
| 11 | Security-regression / iterate-degrades-security arXiv:2506.11022 (failure-mode table) | CONFIRMED | "Security Degradation in Iterative AI Code Generation," peer-reviewed/accepted IEEE-ISTAS 2025; +37.6% critical vulns after 5 iterations. https://arxiv.org/abs/2506.11022 |
| 11 | Same finding, automated-layer prose | CONFIRMED | Same source; "IEEE-ISTAS 2025" added inline. |
| 11 (ref) | Back-references: Veracode 2025; arXiv:2506.11022; Zheng MT-Bench | CONFIRMED | Veracode: 45% AI code failed security tests, Java ~72% (2025 GenAI Code Security Report). MT-Bench: Zheng et al. **2023** (arXiv:2306.05685), GPT-4 ~80% human agreement (corrected year 2024→2023). |
| 13 | Veracode + arXiv:2506.11022 back-refs (Ch.3) | CONFIRMED | Resolved to home-chapter settlement: Veracode 45%/Java 72%; 2506.11022 IEEE-ISTAS 2025. |
| 13 | Lab-to-prod gap arXiv:2511.14136; AgentBench (Liu 2023) | CONFIRMED | 2511.14136 = "Beyond Accuracy: A Multi-Dimensional Framework..." (AWS). Lab-to-prod figure is **60% single-run → 25% 8-run consistency**, NOT a 37% gap — resolved to run-consistency framing per home-chapter settlement. AgentBench = arXiv:2308.03688, ICLR 2024. |
| 13 | Zheng et al. MT-Bench ~80% (Ch.8) | CONFIRMED | Zheng et al. 2023, arXiv:2306.05685, GPT-4 ~80% human agreement. Year corrected 2024→2023. |
| 10/13 | "27% more issues missed (Wang et al. 2024, d=0.62)" automation-bias figure | UNVERIFIED — NOT ASSERTED | No traceable primary source; confabulated. Not present in any chapter body; CH13 reference note explicitly states it is NOT carried, direction rests on Lyell & Coiera 2017 + Beck et al. 2025. |

## Notes
- Beck et al. 2025 (arXiv:2509.08514) already labeled **preprint** in CH10 — unchanged, correct.
- All resolvable flags web-verified via WebSearch (ACM DL, arXiv, SAGE, Veracode, CSET). No fetches blocked.
- The 27% figure is confirmed **not asserted as fact** anywhere in chapters 10, 11, or 13.

---

