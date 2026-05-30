# TIKTOC — Validating Output from AI Systems

*Tik TOC canonical planning document · 2026-05-29*
*Book type: course textbook / professional course (also a practitioner reference). Target 13 primary chapters. Chapter = week (15-week semester).*

---

## Place in the series (read first)

This is the **third book** in the prompt-engineering series and the **cross-cutting verification volume**. The others teach you to *produce* AI output; this one teaches you to *trust* it.

- *Prompt Engineering with LLMs* — chat prompting (how to get output).
- *Prompt Engineering with CLIs* — the discipline of CLI agents (how to make an agent reliable).
- *Prompt Engineering for CLI AI Coding Agents* — the software-engineering practice (how to ship code with agents).
- ***Validating Output from AI Systems*** (this book) — **how do you know the output is correct?** The validation layer underneath all three.

**Overlap is deliberate and one-directional.** The coding-agents book touches code validation (TDD, review, security) and agentic trajectories in passing; *this* book is the deep, output-type-agnostic treatment those chapters point to. Where the other books say "verify it," this book says *how* — and names where verification fails. Cross-reference, don't duplicate: the other books own the *generation* workflow; this one owns the *validation* method.

---

## Book Concept Summary

This book teaches **how to validate the output of AI systems — code, factual claims, reasoning, structured data, and agentic task execution — using layered, deterministic-first defenses, and how to recognize where validation breaks down** to **engineers, reviewers, and technical leads who ship or sign off on AI-generated output and need to know it is correct, not merely plausible**, by **working output type by output type, naming what each validation technique catches and what it misses, and ending at the unsolved frontier of scalable oversight**. It fills the gap left by generation-focused guides, which assume that producing output is the hard part. It succeeds if the reader can **design a validation pipeline appropriate to an output type and risk level, and state honestly what that pipeline cannot catch.**

- **Title:** Validating Output from AI Systems
- **Author:** Nik Bear Brown
- **Logline:** Plausible is not correct. This book is the method for telling the difference — by output type, layer by layer, and an honest map of where no method yet works.
- **Central thesis:** **Validation works where ground truth is mechanically available (tests, compilers, schemas, citations that exist or don't) and fails where it isn't (reasoning quality, factual accuracy in novel domains, agentic correctness) — and the fluency of AI output actively defeats human reviewers.** So the engineering discipline is: deterministic validators first, layered defenses by output type, and explicit acknowledgment of the scalable-oversight gap.

## Learner Profile

Primary reader: an engineer, QA lead, ML engineer, or domain reviewer who already uses AI to produce work and is now responsible for its correctness — in code review, in a RAG product, in an agentic pipeline, or in a high-stakes domain (legal, medical, finance). Knows software testing and CI; has felt the pull to accept a fluent answer. **Misconception to dislodge:** that a confident, well-structured answer is evidence of accuracy, and that "the model checked its work" means anything. Reads to ship safely and to know the limits of what they can certify.

## Book Type & Deployment

Course textbook, primary — a verification/AI-quality course; pairs as the capstone course after the generation books. Secondary: a practitioner reference consulted by output type. **Not** a prompting book, **not** a generic software-testing book (it is specifically about *AI* output and why it's different), **not** an AI-safety theory monograph (scalable oversight is one chapter, treated as the open frontier, not the whole book).

## Sequencing Model

**Concrete-to-abstract / problem-to-solution, organized by output type.** Act One establishes why AI output is a different validation problem and plants the deterministic-first principle. Act Two walks the five output types (code → factual → reasoning → structured → agentic), each "what it catches / what it misses." Act Three covers the *evaluators* (LLM-as-judge, PRMs, humans), assembles a pipeline, and ends at the unsolved frontier. First deterministic-validation artifact lands in Ch. 2.

## Three-Act Learning Arc

This book takes the reader from *"the answer looked right, so I shipped it"* to *"I built a layered validation pipeline for this output type and I can state what it cannot catch"* by first **establishing why AI-output validation differs from software validation and why deterministic checks come first** (Act One, Ch. 1–2), then **validating each output type with honest catch/miss accounting** (Act Two, Ch. 3–7), then **examining the evaluators (machine and human), assembling a pipeline, and confronting scalable oversight** (Act Three, Ch. 8–13).

- **Transition 1→2:** reader accepts that fluency ≠ accuracy and that deterministic validators come first, and now applies validation by output type.
- **Transition 2→3:** reader can validate any single output type and now reasons about *who/what does the judging* and the limits when ground truth is gone.

---

## Chapter Set (13 primary chapters)

### ACT ONE — The Validation Problem

**Ch. 1 — Why Validating AI Output Is a Different Problem**
- Capability (Understand): explain the asymmetry (generation easier than verification), the fluency problem (well-formed ≠ correct), compounding error in agentic systems, and where validation responsibility sits (model / app layer / infra / human).
- Source: synthesis §1. Bridge → Ch. 2 (start where judgment isn't needed).

**Ch. 2 — The Deterministic Floor: Validators That Don't Need Judgment**
- Capability (Apply): use compilers, typecheckers, schema validators, linters, and SAST as the first, cheapest, unambiguous validation layer — the "deterministic validators first" principle.
- Content: why external + deterministic feedback beats model judgment; what this layer reliably catches; why it's non-negotiable in CI.
- Source: §2 (compiler grounding, static analysis) + structured-output framework. Bridge → Ch. 3.

### ACT TWO — Validating by Output Type

**Ch. 3 — Validating Generated Code**
- Capability (Evaluate): build a layered code-validation stack (compiler → tests → SAST → bounded diff review) and explain why iterative LLM "self-improvement" degrades security.
- Content: Veracode 2025 (45% of AI code failed security tests; Java 72%) [verify]; the 37.6% vulnerability increase after 5 refinement iterations (IEEE-ISTAS 2025, arXiv:2506.11022) [verify]; tests validate the spec not the intent; mutation testing; diff review scope limits. **Overlap: deep version of the coding-agents book's TDD/review/security chapters.**
- Source: §2. Bridge → Ch. 4.

**Ch. 4 — Validating Factual Claims**
- Capability (Evaluate): apply RAG grounding, citation-existence + claim-support checking, and claim decomposition — and explain why RAG shifts hallucination into retrieval error rather than eliminating it.
- Content: span-level hallucination (SemEval-2025 Mu-SHROOM); RAG reduces but doesn't solve (NAACL 2024 Ayala & Bechard; MEGA-RAG ~40% reduction) [verify]; citation exists ≠ citation correct; the legal-brief fabricated-cases failure; high-stakes domains require expert review.
- Source: §3. Bridge → Ch. 5.

**Ch. 5 — Validating Reasoning Chains**
- Capability (Evaluate): choose verification by domain — symbolic/execution/proof checkers for formal domains; self-consistency (sample + majority vote) over self-critique elsewhere; PRMs for step-level where available.
- Content: why ungrounded self-correction hurts (cross-ref Ch. 8/9); self-consistency vs self-critique; the limit when "correct intermediate step" is ambiguous.
- Source: §3 (reasoning) + framework. Bridge → Ch. 6.

**Ch. 6 — Validating Structured Output**
- Capability (Apply→Evaluate): enforce structure (schema validation: Pydantic/Zod/JSON Schema), then validate semantics (right values, not just valid shape); prefer API-level structured output to prompt-only; use retry loops without treating them as enforcement.
- Source: framework (structured output). Bridge → Ch. 7.

**Ch. 7 — Validating Agentic Task Execution**
- Capability (Evaluate): evaluate the *trajectory* (sequence of actions), not just final output; checkpoint before irreversible actions; catch loops, scope violations, wrong-tool, error propagation.
- Content: the lab-to-production gap (37% for multi-agent, arXiv:2511.14136) [verify]; trajectory evaluation (AgentBench, Liu et al. 2023; WebArena VLM-judge 74–93% agreement) [verify]; the irreversibility problem → validate before destructive actions. **Overlap: deep version of the coding-agents book's review/CI chapters.**
- Source: §6. Bridge → Ch. 8 (who does the judging?).

### ACT THREE — The Evaluators and the Limits

**Ch. 8 — LLM-as-Judge: Uses and Failure Modes**
- Capability (Evaluate): use LLM judges for ranking/screening and reject them for close calls; name and test for position/verbosity/self-preference/style bias and the circularity problem.
- Content: GPT-4 ~80% MT-Bench correlation — what it does and doesn't mean (Zheng et al. 2024); the bias catalog (Shi et al. arXiv:2406.07791); some biases now negligible in current models (arXiv:2604.23178 [verify]); multi-judge ensembles; shared-failure-mode circularity.
- Source: §4. Bridge → Ch. 9.

**Ch. 9 — Process Reward Models: Step-Level Verification**
- Capability (Evaluate): use PRMs for step-level verification in formal domains; explain why they beat ORMs and LLM-as-judge on reasoning, and where they generalize poorly.
- Content: PRMs vs ORMs (Lightman et al. 2024); automated PRM training via MC rollouts (Wang et al. 2024; ~1–2% RL gain but stronger as test-time verifiers); ThinkPRM (arXiv:2504.16828) — minimal labels beat discriminative PRMs; the open-ended-task limit.
- Source: §7. Bridge → Ch. 10.

**Ch. 10 — Human Validation and Automation Bias**
- Capability (Evaluate→Create): design review protocols that counter automation bias — cognitive forcing functions, adversarial framing, bounded scope, fatigue-aware approval design.
- Content: 27% more issues missed reviewing AI vs independent (Wang et al. 2024, d=0.62) [verify]; Lyell & Coiera clinical automation bias; skeptics catch more, incentives don't fix it; approval fatigue in agentic systems; cognitive forcing > explainability.
- Source: §5. Bridge → Ch. 11.

**Ch. 11 — Building a Validation Pipeline**
- Capability (Create): assemble a layered, output-type-appropriate validation pipeline (deterministic → automated → judge/PRM → human) wired into CI with observability; map the common failure modes to the layer that catches each.
- Content: the by-output-type framework as an architecture; the failure-mode table (fluency-as-proxy, tests-that-don't-test, RAG retrieval error, self-eval loops, security regression, approval fatigue, benchmark contamination, judge circularity) and their mitigations.
- Source: "Validation by Output Type" framework + failure-modes table + takeaways. Bridge → Ch. 12.

**Ch. 12 — Scalable Oversight: The Unsolved Problem**
- Capability (Understand→Evaluate): explain why human validators increasingly cannot verify frontier output, and assess current research directions honestly.
- Content: the asymmetry at the frontier; debate (early-stage, no reliable scaling); weak-to-strong generalization (Burns et al., OpenAI); task decomposition; PRMs as partial answer; the honest verdict — unsolved. Mark this the book's most contested, fastest-moving chapter.
- Source: §8. Bridge → Ch. 13.

**Ch. 13 — Capstone: Designing a Validation Strategy for a Real System** *(synthesis)*
- Capability (Create): given an output type and a risk profile, design and justify a complete validation strategy — the layers, what each catches, the residual risk a human must own, and what cannot be certified.
- Source: assembles the whole book; a concrete, labeled scenario (e.g. a RAG product or an agentic code pipeline).

---

## Bloom's Distribution (13 primary chapters)
- Understand: Ch. 1 (Ch. 12 Understand→Evaluate)
- Apply: Ch. 2, 6
- Evaluate/Create: Ch. 3, 4, 5, 7, 8, 9, 10, 11, 13
Eleven of thirteen anchor at Apply or above; first hands-on artifact (deterministic validation) by Ch. 2.

## Contested Claims / Aging-Risk Audit
- **Practitioner + early-research mix** — preface caveat; `[verify]` on the quantitative claims (45% / 72% Veracode; 37.6% iteration increase; 27% review miss; 37% lab-to-prod gap; MT-Bench ~80%; MEGA-RAG ~40%).
- **Ch. 8 (LLM-as-judge)** — bias profiles change as models improve (the "some biases now negligible" finding); teach the *method of testing for bias*, not a fixed bias list.
- **Ch. 9 (PRMs) & Ch. 12 (scalable oversight)** — fastest-moving research; separate the stable problem statement from current-state results; expect both to age within 2–3 years.
- **Ch. 3 (code security stats)** — model-generation-specific; dated snapshot, stable principle (deterministic-first, don't-iterate-for-security).

## Out of Scope (this edition)
- Generation/prompting workflows (→ the three companion books; this book begins where output already exists).
- Generic non-AI software QA theory (covered only where AI output differs).
- Full AI-alignment theory (scalable oversight is Ch. 12, framed as the open frontier, not the book's subject).
- Model training / RLHF internals (→ *Prompt Engineering with LLMs* Part IV).

## Source Inventory (seeds for the research pass)
- Primary spine: the author-supplied "Validating Output from AI Systems: Research Synthesis" (May 2026), §§1–8 + the by-output-type framework + failure-mode table.
- Key sources to verify/expand: Veracode 2025 GenAI Code Security Report; IEEE-ISTAS 2025 (arXiv:2506.11022); arXiv:2510.26103 (GitHub code-vuln analysis); Zheng et al. 2024 (MT-Bench); Shi et al. (arXiv:2406.07791, position bias); arXiv:2604.23178 (judge debiasing) [verify future-dated]; Lightman et al. 2024 + Wang et al. 2024 (PRMs); ThinkPRM (arXiv:2504.16828); Ayala & Bechard NAACL 2024 (RAG); SemEval-2025 Task 3 Mu-SHROOM; Wang et al. 2024 (automation bias, 27%); Lyell & Coiera (automation bias review); Burns et al. OpenAI (weak-to-strong); AgentBench (Liu et al. 2023); WebArena; arXiv:2511.14136 (lab-to-prod gap). Note: the coding-agents book's `pantry/research-ch-*` (TDD, review, evaluation, security) and `research-ch-14-cli-coding-agents.md` are adjacent prior art for Ch. 3 and Ch. 7.

---
*Next step: Chapter Research Pass → one `pantry/research-ch-NN-slug.md` per chapter. Highest-value research is Act Two (output-type-specific) and Ch. 8–9, 12 (evaluators + oversight). To pressure-test: `/g2`, `/looptest`, `/m1` (positioning vs. the three companions) in interactive mode.*
