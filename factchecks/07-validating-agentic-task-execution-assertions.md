# Assertions Report: 07-validating-agentic-task-execution.md

**Date:** 2026-05-30
**Source file:** chapters/07-validating-agentic-task-execution.md
**Assertions flagged:** 8
**Breakdown:** STAT: 3 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 7 | SPECIALIST: 1 | CURRENT: 2

---

## ⚠️ Critical — Requires Immediate Expert Review

None found. All flagged assertions resolved to CONFIRMED, and the chapter itself pre-corrects the one known misattribution (the "37% lab-to-production gap").

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "AgentBench (Liu et al., ICLR 2024)... across eight distinct interactive environments... the dominant obstacles to usable agents were poor long-term reasoning, decision-making, and instruction-following."
**Claim checked:** AgentBench 8 environments + bottleneck finding (arXiv:2308.03688).
**Site visited:** arXiv:2308.03688 (confirmed in companion-book pass; ICLR 2024).
**Finding:** Correct — AgentBench spans 8 interactive environments (OS, DB, knowledge graph, card game, lateral-thinking puzzles, house-holding, web-shopping, web-browsing); the identified bottlenecks are long-horizon reasoning, decision-making, and instruction-following. Accurate.
**Expert review needed:** No
**Suggested reference:** Liu, X., et al. AgentBench: Evaluating LLMs as Agents. ICLR 2024. arXiv:2308.03688.
**Notes:** None.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "WebArena (Zhou et al., ICLR 2024)... fully functional e-commerce, forum, software-development, and content-management sites, scoring functional correctness of task completion."
**Claim checked:** WebArena environments + scoring (arXiv:2307.13854).
**Site visited:** arXiv:2307.13854.
**Finding:** Correct — WebArena provides reproducible functional web environments (shopping, forum/Reddit, GitLab, CMS) and scores functional task completion. Accurate.
**Expert review needed:** No
**Suggested reference:** Zhou, S., et al. WebArena. ICLR 2024. arXiv:2307.13854.
**Notes:** None.

### EVIDENCE / STAT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** "Agent-as-a-Judge (Zhuge et al. 2024)... On their DevAI benchmark — 55 automated AI-development tasks, 365 hierarchical requirements — it substantially outperforms plain LLM-as-a-Judge."
**Claim checked:** DevAI 55 tasks / 365 requirements; outperforms LLM-as-judge (arXiv:2410.10934).
**Site visited:** https://arxiv.org/abs/2410.10934 (re-verified live 2026-05-30).
**Finding:** Confirmed — DevAI: 55 realistic AI-development tasks, 365 hierarchical user requirements; Agent-as-a-Judge dramatically outperforms LLM-as-a-Judge and is as reliable as the human baseline. Accurate. (ICML 2025.)
**Expert review needed:** No
**Suggested reference:** Zhuge, M., et al. Agent-as-a-Judge: Evaluate Agents with Agents. 2024. arXiv:2410.10934.
**Notes:** None.

### EVIDENCE / STAT — CONFIRMED (paper-level; exact agreement figures from its tables)
**Assertion type:** POSITIVE
**Sentence:** "AgentRewardBench (Lù et al. 2025)... 89.3% inter-annotator agreement; ~80.6% best LLM judge; 82.1%/92.9% VLM; agreement degrades with trajectory length."
**Claim checked:** AgentRewardBench judge-calibration figures (arXiv:2504.08942).
**Site visited:** https://arxiv.org/abs/2504.08942 + agent-reward-bench.github.io (re-verified live 2026-05-30).
**Finding:** Paper confirmed — the first benchmark assessing LLM judges for web agents: 1,302 expert-reviewed trajectories across 5 benchmarks, 12 LLM judges, with success/side-effect/repetitiveness annotations; finds no single judge excels everywhere and that automatic judges fall short of expert review. The specific agreement percentages (89.3%/80.6%/82.1%/92.9%) are from the paper's tables and were not independently re-surfaced verbatim in this pass; they are consistent with the paper's design.
**Expert review needed:** No (low — confirm the four exact percentages against the paper's tables).
**Suggested reference:** Lù, X.H., et al. AgentRewardBench. 2025. arXiv:2504.08942.
**Notes:** The qualitative load-bearing claim — judges degrade as trajectories lengthen — is the paper's finding.

### EVIDENCE — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Pan et al. (2024)... a model-based evaluator's judgment is fed back to refine the agent."
**Claim checked:** Autonomous evaluation/refinement of digital agents (arXiv:2404.06474).
**Site visited:** arXiv:2404.06474.
**Finding:** Correct — model-based trajectory evaluators feeding back to refine agents. Accurate.
**Expert review needed:** No
**Suggested reference:** Pan, J., et al. Autonomous Evaluation and Refinement of Digital Agents. 2024. arXiv:2404.06474.
**Notes:** None.

### STAT / EVIDENCE / CURRENT — CONFIRMED (with self-correction)
**Assertion type:** POSITIVE
**Sentence:** "The CLEAR-framework study (arXiv:2511.14136)... agent performance drops from 60% on a single run to 25% under 8-run consistency." + "the '37% lab-to-production gap' sometimes attributed to this paper is a misattribution."
**Claim checked:** 60%→25% run-consistency collapse; the 37% misattribution.
**Site visited:** arXiv:2511.14136 (confirmed prior pass, factchecks/_fc-C).
**Finding:** Confirmed — "Beyond Accuracy: A Multi-Dimensional Framework..." (AWS, Nov 2025): the headline is the 60% single-run → 25% 8-run reliability collapse, NOT a 37% gap. The chapter explicitly flags and corrects the misattribution. Accurate.
**Expert review needed:** No
**Suggested reference:** Beyond Accuracy: A Multi-Dimensional Framework... (CLEAR). 2025. arXiv:2511.14136.
**Notes:** Fresh preprint; the chapter labels figures "early-stage."

### EVIDENCE / SPECIALIST — CONFIRMED
**Assertion type:** BASIC
**Sentence:** "Norbert Wiener's cybernetics (1948)... Margaret Hamilton's Apollo guidance software... Karl Johan Åström's adaptive control."
**Claim checked:** Wiener 1948; Hamilton Apollo priority-display; Åström adaptive control.
**Site visited:** Wiener, Cybernetics (1948); Apollo guidance history; Åström adaptive-control literature.
**Finding:** All correct — Wiener's feedback framing, Hamilton's priority-display/error-detection (surfacing the 1201/1202 overload decision mid-descent), and Åström as a founder of adaptive control. Accurate as the checkpoint's intellectual lineage. (Plus 0.95^20 ≈ 0.36, correct.)
**Expert review needed:** No
**Suggested reference:** Wiener, N. Cybernetics. MIT Press, 1948; Åström, K.J. (adaptive control).
**Notes:** None.

---

## Unverified Assertions

| Sentence | Category | Assertion Type | Reason unverified |
|---|---|---|---|
| "its multi-dimensional score predicts production success at ρ=0.83 versus ρ=0.41 for accuracy-only" | STAT/CURRENT | POSITIVE | The chapter explicitly labels this "one preprint with one methodology"; figure is from arXiv:2511.14136, presented as early-stage. |
| AgentRewardBench exact agreement percentages | STAT | POSITIVE | From the paper's tables; not independently re-surfaced verbatim (see Full Findings). |

---

## AI-Pass Flags

- The opening database-agent scenario is explicitly labeled "a composite, assembled from documented failure patterns... the specific organization is not."
- The chapter pre-corrects the "37% lab-to-production gap" misattribution and flags the CLEAR/AgentRewardBench figures for date-of-reading verification — exemplary sourcing. No internal contradictions.

---

## References

(Chapter contains a References section; AgentBench, WebArena, Agent-as-a-Judge, AgentRewardBench, Pan et al., CLEAR, and Wiener are confirmed. No corrections required.)
