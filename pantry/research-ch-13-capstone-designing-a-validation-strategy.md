# Research: Chapter 13 — Capstone: Designing a Validation Strategy for a Real System
## Validating Output from AI Systems
**Chapter one-line:** Given an output type and a risk profile, design and justify a complete validation strategy — the layers, what each catches, the residual risk a human must own, and what cannot be certified.
**Research date:** 2026-05-29

---

## 1. Primary Sources

> This is a **synthesis chapter**. It introduces no new primary literature; it assembles the sources already established in Ch. 1–12 into a single worked design. Citations below are the *load-bearing prior sources* the capstone leans on, grouped by the layer they justify. Full treatment of each lives in its home chapter.

### Foundational papers and texts (by validation layer, all established earlier in the book)

- **Deterministic floor (Ch. 2):** compilers, typecheckers, schema validators, linters, SAST as the first, judgment-free layer. Home chapter: Ch. 2. The capstone's first move for *every* output type.
- **Code (Ch. 3):** layered code stack (compile → test → SAST → bounded diff review); Veracode 2025 GenAI Code Security (~45% of AI code failed security tests; Java ~72%) [verify]; iteration-degrades-security (arXiv:2506.11022, ~37.6% vuln increase after 5 refinements) [verify]. Tests validate the spec, not the intent.
- **Factual claims (Ch. 4):** RAG grounding + citation-existence + claim-support checking; Ayala & Bechard NAACL 2024 (RAG reduces but does not solve hallucination); SemEval-2025 Mu-SHROOM (span-level hallucination); the legal-brief fabricated-cases failure. "Citation exists ≠ citation correct."
- **Reasoning (Ch. 5):** self-consistency (Wang et al. 2022, arXiv:2203.11171) over self-critique (Huang et al. 2024, arXiv:2310.01798 — "LLMs cannot self-correct reasoning yet"); execution/proof oracles (PAL, Lean/Coq) where available.
- **Structured output (Ch. 6):** schema validation (Pydantic/Zod/JSON Schema) for shape, then *semantic* validation for values; prefer API-level structured output to prompt-only.
- **Agentic execution (Ch. 7):** validate the *trajectory*, not just the final output; checkpoint before irreversible actions; lab-to-prod gap (arXiv:2511.14136, ~37% for multi-agent) [verify]; trajectory eval (AgentBench, Liu et al. 2023; WebArena VLM-judge 74–93% agreement) [verify].
- **LLM-as-judge (Ch. 8):** judges for ranking/screening, not close calls; Zheng et al. 2024 (MT-Bench ~80% GPT-4/human agreement) [verify]; Shi et al. (arXiv:2406.07791, position/verbosity/self-preference bias); the circularity problem.
- **PRMs (Ch. 9):** step-level verification in formal domains; Lightman et al. 2023/2024 (arXiv:2305.20050, process > outcome supervision); ThinkPRM (arXiv:2504.16828); open-ended-task limit.
- **Human validation (Ch. 10):** cognitive forcing functions, adversarial framing, bounded scope; ~27% more issues missed reviewing AI vs independent (Wang et al. 2024, d=0.62) [verify]; Lyell & Coiera (clinical automation bias).
- **Pipeline assembly (Ch. 11):** the by-output-type framework as architecture; the failure-mode table mapped to the layer that catches each.
- **Scalable oversight (Ch. 12):** the frontier where the human oracle is outrun; debate / weak-to-strong / decomposition / PRMs as *partial* answers; honest verdict — unsolved. This is the source of the capstone's mandatory **residual-risk / cannot-certify** statement.

### Key empirical cases (the scenarios the capstone designs against)
- **Scenario A — a customer-facing RAG product** (e.g., a support/answers assistant over a documentation corpus). Output types: *factual claims* (primary) + *structured output* (citations, metadata) + occasional *reasoning*.
- **Scenario B — an agentic code pipeline** (an autonomous coding agent that edits a repo, runs tools, and opens PRs). Output types: *generated code* (primary) + *agentic task execution* (the trajectory) + *structured output* (tool calls).
- These two are the TIKTOC's named candidate scenarios; the chapter should fully develop **one** (recommended: Scenario A for breadth across factual/structured/judge layers, with Scenario B as the contrasting sidebar) and provide the other as an exercise.

---

## 2. The Core Concept — State of the Field

### What is settled
- **A validation strategy is determined by output type × risk profile, not by tooling preference.** The book's whole architecture (Ch. 11) is that you read off the layers from the output type, then set their *strictness* from the risk profile.
- **Deterministic-first is non-negotiable.** Every strategy starts with the cheapest judgment-free layer that applies (Ch. 2) before any model-based or human layer.
- **No single layer is sufficient; layers catch different failure classes.** The failure-mode-to-layer mapping (Ch. 11) is the settled organizing device.
- **Every honest strategy ends with a residual-risk statement.** Because validation fails where ground truth isn't mechanically available (Ch. 12), a complete strategy *must* name what it cannot certify and assign that risk to a human owner.

### What is disputed
- **How much to trust LLM-as-judge / model-based layers in the final gate** (Ch. 8 circularity) — the capstone must take a position (use for screening, never as the sole high-stakes gate) while acknowledging it is contested.
- **Where the human-in-the-loop boundary sits** as models improve (Ch. 10, Ch. 12) — the line moves, so the strategy must be written to make it *explicit and adjustable*, not fixed.
- **Whether agentic trajectories can be validated well enough for autonomous high-stakes action** (Ch. 7, Ch. 12) — the capstone should default to checkpoint-before-irreversible rather than full autonomy.

### What has changed recently (last 5 years)
- The field moved from "test the final output" to **layered, output-type-specific** validation wired into CI (the book's thesis-in-practice).
- Structured-output and tool-use APIs made the *structured* layer cheaper and more deterministic (Ch. 6).
- Agentic systems made **trajectory validation** a first-class concern (Ch. 7), and the lab-to-prod gap made "it worked in the demo" an explicit risk.
- Scalable-oversight research (Ch. 12) sharpened the honest upper bound: as capability rises, the residual-risk box grows, and the capstone must say so.

---

## 3. Application Domain Examples

**Worked scenario (recommended primary): a customer-facing RAG support assistant.**
*Labeled as a hypothetical design exercise.* Risk profile: medium-high (wrong answers reach customers, but no irreversible physical/financial action; reputational + occasional compliance risk). Output types present: factual claims (dominant), structured citations/metadata, light reasoning.

Layered strategy, read off the framework:
1. **Deterministic floor (Ch. 2 / Ch. 6):** JSON-schema-validate the response envelope (answer + citation list + confidence); reject malformed responses before anything else. Catches: shape errors, missing-citation responses. Misses: everything semantic.
2. **Citation-existence check (Ch. 4):** every cited document ID must resolve to a real chunk in the corpus. Deterministic. Catches: fabricated/hallucinated citations (the legal-brief failure mode). Misses: real citation that doesn't support the claim.
3. **Claim-support / grounding check (Ch. 4):** decompose the answer into claims; verify each is entailed by its cited span (entailment model or retrieval-overlap heuristic). Catches: unsupported claims, retrieval-error answers. Misses: subtly wrong paraphrase; correct-but-incomplete answers — and shifts hallucination into *retrieval* error rather than eliminating it.
4. **LLM-as-judge screen (Ch. 8):** a judge model flags low-confidence / likely-unhelpful answers for human review — used for *screening only*, never as the sole correctness gate (circularity, self-preference bias). Catches: gross quality misses at scale. Misses: shared-failure-mode errors (judge and generator wrong the same way).
5. **Human review (Ch. 10), risk-gated:** answers in compliance-sensitive categories (refunds, legal, medical-adjacent) routed to a human with a cognitive-forcing checklist and bounded scope. Catches: high-stakes semantic errors. Misses: whatever the fatigued/automation-biased reviewer waves through (~27% more misses on AI output [verify]).

**Contrasting sidebar: an agentic code pipeline.** Same method, different layers: deterministic floor = compile + typecheck; code layer = tests + SAST + bounded diff review (Ch. 3), with the *don't-iterate-for-security* rule; agentic layer = trajectory evaluation + **checkpoint before any irreversible action** (force-push, deploy, data migration) per Ch. 7; human gate on the diff, not the whole session. Residual risk concentrates in trajectory correctness and the spec-vs-intent gap (tests validate the spec, not whether the spec was right).

---

## 4. The Book's Thesis Connection
The capstone *is* the thesis operationalized. The reader is handed an output type and a risk level and must produce: (1) the layer stack, deterministic-first; (2) a catch/miss accounting for each layer; (3) the residual risk a named human owns; and (4) an explicit **cannot-certify** statement. Steps (1)–(2) are "validation works where ground truth is mechanically available" — every deterministic and grounded layer is an instance of an available oracle (schema, citation-existence, compiler, test). Steps (3)–(4) are "and fails where it isn't" — the part of the output where no oracle exists (was the answer *right and complete*? was the agent's *plan* sound? is the reasoning *valid* in a novel domain?) is exactly the residual-risk box, and Ch. 12 is why it cannot be closed. The chapter's success condition (from TIKTOC) is precisely the book's: the reader can design the pipeline *and state honestly what it cannot catch.*

---

## 5. The AI Wayback Machine — Candidate Figures

- **W. Edwards Deming** (full name; Wikipedia: "W. Edwards Deming"). Statistician of quality control; "you cannot inspect quality into a product — it has to be built in," and the insistence on *layered process control* over end-of-line inspection. The capstone's deepest analogy: validation is not a final gate but a system of layered, designed-in checks — and Deming's distinction between common-cause and special-cause variation maps onto deterministic (special-cause, catchable) vs frontier (common-cause, residual) error. Lesser-known to engineers than to manufacturing; strong fit. **Anchor prompt:** *"A factory line with checks built into every station rather than one inspector at the end — quality designed into the process, not bolted on."* Skew flag: 20th-century American male.

- **James Reason** (full name; Wikipedia: "James Reason"). Psychologist; the **Swiss-cheese model** of accident causation — defenses are layers, each with holes, and a failure occurs only when holes align. This *is* the capstone's layered-defense picture, and it names the residual risk honestly: no layer is whole; you manage aligned-hole probability, you don't eliminate it. Pairs perfectly with the failure-mode-to-layer table. Lesser-known by name than the model itself. **Anchor prompt:** *"Several slices of Swiss cheese stacked as layers of defense; a single hazard passes only when the holes in every layer line up."* Skew flag: British male; diversifies discipline (human-factors/safety) away from CS.

- **Genichi Taguchi** (full name; Wikipedia: "Genichi Taguchi"). Engineer; *robust design* and the **loss function** — quality is loss imparted to society from variation, and you design systems to be insensitive to the noise you can't remove. The capstone's risk-profile lever: how much residual variation is *tolerable* is a design parameter set by stakes, exactly as Taguchi framed tolerance design. Adds non-Western nationality to the set. **Anchor prompt:** *"An engineer tuning a process to be insensitive to the noise it cannot eliminate, with a loss curve rising sharply away from target."* Skew flag: Japanese male — diversifies nationality.

Skew note: three quality/safety-engineering figures, all 20th-century men; the deliberate move is to pull the capstone's anchors *out of CS* into manufacturing quality (Deming, Taguchi) and human-factors safety (Reason), since the capstone's real subject is layered-defense design, which those fields formalized first. None reused from earlier chapters. Taguchi adds nationality diversity; gender diversity remains a gap in this lineage — flagged.

---

## 6. Pedagogical Delivery Research
- **Make it a fill-in-the-blanks design template, not prose.** The reader should leave with a reusable artifact: *Output type → layers (deterministic-first) → catch/miss per layer → residual risk → human owner → cannot-certify statement.* The chapter teaches by having the reader complete it for a given scenario.
- **Work one scenario end-to-end on the page, assign the other.** Develop the RAG assistant fully (it exercises the most layers: deterministic, factual, structured, judge, human); leave the agentic code pipeline as the graded exercise so the reader transfers the method.
- **Force the residual-risk statement.** The single most important pedagogical beat: a strategy without an explicit "here is what this cannot certify, and here is the human who owns it" is incomplete and should be marked wrong. This is where the book's honesty becomes the student's habit.
- **Tie strictness to risk, visibly.** Show the *same* output type at two risk levels (low: deterministic + judge-screen only; high: add grounding check + mandatory human gate + checkpointing) so the reader sees risk profile as the dial, not a vibe.
- **Callback discipline.** Every layer should cite its home chapter so the capstone reads as assembly, reinforcing retention of the whole book.

---

## 7. Representation and Display Research
**End-to-end validation-strategy decision flow (specified).** A single decision diagram the reader runs for any system:

1. **Identify the output type(s)** the system produces — code / factual / reasoning / structured / agentic (a system usually has more than one; handle each).
2. **For each output type, set the risk level** from impact × reversibility (low / medium / high; "irreversible action" forces high).
3. **Lay the deterministic floor first (Ch. 2):** which judgment-free checks apply (compile, typecheck, schema, lint, SAST, citation-existence)? *Always present.*
4. **Add the output-type layer(s):**
   - code → tests + SAST + bounded diff review (Ch. 3)
   - factual → RAG grounding + claim-support (Ch. 4)
   - reasoning → self-consistency / execution / PRM (Ch. 5, Ch. 9)
   - structured → semantic value validation beyond schema (Ch. 6)
   - agentic → trajectory eval + checkpoint-before-irreversible (Ch. 7)
5. **Add an evaluator layer only where justified:** LLM-as-judge for *screening* (Ch. 8); PRM for step-level in *formal* domains (Ch. 9). Never the sole high-stakes gate.
6. **Set the human gate by risk (Ch. 10):** none / spot-check / mandatory-with-forcing-function; design against automation bias and fatigue.
7. **Write the residual-risk statement (Ch. 12):** what no layer catches (e.g., correct-but-incomplete answers, sound-looking-but-wrong plans, novel-domain reasoning), who owns it, and the explicit **cannot-certify** line.

Render as a top-to-bottom flow with the deterministic floor as a mandatory first band, output-type layers as a middle band that varies, and the residual-risk box drawn *deliberately uncloseable* (open-bottomed) to make the thesis visual: the box never fully closes, and its size grows with capability (Ch. 12). Recommend a paired before/after of the same output type at low vs high risk to show the dial.

---

## 8. Open Questions and Research Gaps
- **How to size the residual-risk box.** There is no accepted method to *quantify* what a layered pipeline cannot catch for a given output type — the capstone teaches naming it, not measuring it, because the measurement doesn't reliably exist (Ch. 12).
- **When does layering hit diminishing returns or shared failure modes?** Stacking model-based layers (judge + PRM + agentic-judge) risks correlated errors (Ch. 8 circularity); the optimal stack composition is not characterized.
- **How to keep the human gate effective under volume.** Automation bias and approval fatigue (Ch. 10) degrade exactly the layer the capstone leans on for residual risk; mitigations exist but no validated dosing.
- **Where to set autonomy for agentic systems.** Checkpoint-before-irreversible is the defensible default, but the boundary between "auto-approve" and "human-gate" for agentic actions is an open, risk-dependent design question (Ch. 7, Ch. 12).
- **Strategy drift as models improve.** A strategy correct today may under- or over-validate as capability shifts the ground-truth-availability line; the chapter should teach writing the strategy to be *revisited*, but how often and against what signal is open.

---

## 9. Sourcing Notes
- **No new primary literature introduced** — by design (TIKTOC: Ch. 13 "assembles the whole book; cite the prior chapters' sources rather than new ones"). Every citation above traces to its home chapter (Ch. 2–12) where it is verified in that chapter's research file.
- **Inherited [verify] tags carry forward unchanged:** the quantitative claims (Veracode 45%/72%; iteration 37.6% / arXiv:2506.11022; lab-to-prod 37% / arXiv:2511.14136; MT-Bench ~80%; automation-bias 27% / d=0.62; WebArena 74–93%) keep their [verify] status from their home chapters; the capstone must not present them as freshly confirmed. Do not re-assert any of these as settled in the capstone narrative.
- **Future-dated ID note:** the TIKTOC's `arXiv:2604.23178` (Ch. 8 judge-debiasing) is future-dated and is **not** used here; if the capstone references "some judge biases now negligible," it must inherit Ch. 8's [verify] and the future-date flag.
- **Scenarios are explicitly labeled hypothetical design exercises** — the RAG assistant and agentic code pipeline are illustrative architectures, not descriptions of a specific shipped product. No real system, person, or proprietary pipeline is named or implied.
- **The residual-risk / cannot-certify requirement is sourced to Ch. 12** (scalable oversight, unsolved). It is the one non-negotiable structural element of the capstone and the cleanest expression of the book's thesis.
- **No unverifiable new claim** is made in this chapter; the only verification obligations are inherited and already flagged in the source chapters.
