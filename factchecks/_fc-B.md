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
