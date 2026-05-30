> **Voice status:** `voice-unanchored`. Root `style/` and book-level `style/` empty as of this draft.

---

# Chapter 13 — Capstone: Designing a Validation Strategy for a Real System

*Output type × risk profile in; layers, catch/miss, and an uncloseable residual-risk box out*

**Author:** Nik Bear Brown
**Editor:** Nik Bear Brown

---

## Suggested titles

1. **Capstone: Designing a Validation Strategy for a Real System**
2. **The Strategy Template: Layers, Catch/Miss, Residual Risk, Cannot-Certify**
3. **The Box That Never Closes: Sizing Validation to Risk and Owning What's Left**

---

## TL;DR

This is where the whole book becomes one reusable artifact. Given an **output type** and a **risk profile**, you produce a complete validation strategy: (1) the layer stack, deterministic-first; (2) a catch/miss accounting for each layer; (3) the residual risk a *named human* owns; and (4) an explicit **cannot-certify** statement. Steps (1)–(2) are the thesis's first half — *validation works where ground truth is mechanically available* — every deterministic and grounded layer being an instance of an available oracle (schema, citation-existence, compiler, test). Steps (3)–(4) are the second half — *and fails where it isn't* — the part of the output with no oracle (was the answer right *and complete*? was the agent's *plan* sound? is the reasoning *valid* in a novel domain?) is the residual-risk box, and Ch. 12 is why it cannot be closed. The method is a decision flow: identify output type(s) → set risk from impact × reversibility (irreversible forces high) → lay the deterministic floor (always present) → add the output-type layer(s) → add an evaluator layer only where justified (LLM-judge for screening, PRM for formal-domain steps; never the sole high-stakes gate) → set the human gate by risk → write the residual-risk statement. The chapter works **one** scenario end-to-end (a customer-facing RAG support assistant, a labeled hypothetical) and assigns the other (an agentic code pipeline) as the graded exercise. Two non-negotiables, both habits the book has been building: **risk is the strictness dial** (same output type, two risk levels, two strategies), and **a strategy without an explicit cannot-certify line is incomplete and marked wrong.** The residual-risk box is drawn open-bottomed on purpose — it never fully closes, and it grows with capability (Ch. 12).

---

## Learning objectives

By the end of this chapter you should be able to:

1. **(Apply)** Run the validation-strategy decision flow for a system: enumerate its output types and set each one's risk level from impact × reversibility.
2. **(Create)** Assemble the layer stack for a given output type, deterministic-first, with the specific check in each layer drawn from its home chapter.
3. **(Evaluate)** Write a catch/miss accounting for each layer and identify where the layers run out of mechanical ground truth.
4. **(Create)** Produce the mandatory residual-risk / cannot-certify statement: name what no layer catches, assign a human owner, and tie its size to the risk profile.
5. **(Evaluate)** Tune strictness to risk — show the same output type at two risk levels and justify the difference as a design decision, not a vibe.

## Prerequisites

The whole book. This chapter cites every prior chapter as a building block: Ch. 2 (deterministic floor), Ch. 3 (code), Ch. 4 (factual claims), Ch. 5 (reasoning), Ch. 6 (structured output), Ch. 7 (agentic execution), Ch. 8 (LLM-as-judge), Ch. 9 (PRMs), Ch. 10 (human validation), Ch. 11 (the pipeline as architecture), and — for the residual-risk statement, the one non-negotiable element — Ch. 12 (scalable oversight, the unsolved frontier). If any of those is unfamiliar, the layer it supplies will read as an assertion rather than a tool.

---

## 13.1 The strategy that was just a list of tools

The following is a labeled hypothetical, an illustrative design exercise — not a description of a specific shipped product, person, or proprietary pipeline.

A team was asked to "write the validation strategy" for a new AI feature before launch. They produced a confident document: *We use Pydantic for schema validation. We have a test suite in CI. We run an LLM-as-judge for quality. We have a human review queue. We use SAST.* The list was accurate. Every tool named was a real tool the book has endorsed. The document was signed off, and the feature shipped.

It was not a strategy. It was an inventory. Three things were missing, and their absence is exactly what this chapter exists to install as a habit.

First, it was not indexed to **output type**. "We use SAST" is meaningful for generated code and irrelevant for factual claims; "we have a citation check" is meaningful for a RAG answer and meaningless for a tool call. A strategy reads its layers *off the output type*, and this list did not say which tool defended which kind of output. Second, it had no **catch/miss accounting**. "We run an LLM-as-judge for quality" hides the question that matters — *what does the judge catch, and what does it miss?* (Ch. 8: it screens fluency-correlated quality at scale and shares the generator's blind spots.) A tool named without its holes is a tool you will over-trust. Third, and fatally, it had no **residual-risk statement**: no sentence naming what *none* of the layers catch, who owns that risk, and what the system therefore *cannot certify.* The document implied, by omission, that the listed tools covered everything. They did not — they never do (Ch. 12) — and the omission is the most dangerous thing in it.

A validation strategy is not a list of tools. It is a *defensible argument* of the form: *for this output type, at this risk level, these layers in this order catch these failure classes; what remains uncaught is this, a named human owns it, and here is what we cannot certify.* The rest of this chapter is the template for producing that argument, the worked example that fills it in, and the discipline of ending on the box that never closes.

---

## 13.2 The decision flow

The strategy is the output of a fixed procedure. Run it top to bottom for any system; it composes Ch. 2–12 into seven steps.

1. **Identify the output type(s).** Code / factual / reasoning / structured / agentic. A real system usually produces *more than one* — a RAG product emits factual claims *and* structured citations; an agentic coder emits code *and* a trajectory *and* tool calls. Handle each type separately; they get different layers.

2. **Set the risk level per output type, from impact × reversibility.** Low / medium / high. The rule that overrides everything: **an irreversible action forces high** (Ch. 7 — checkpoint before destructive actions). A wrong answer a user can ignore is lower-risk than a wrong answer that triggers a refund, a deploy, or a data migration. Risk is not a vibe; it is impact times reversibility, written down.

3. **Lay the deterministic floor first (Ch. 2).** Which judgment-free checks apply? Compile, typecheck, schema-validate, lint, SAST, citation-existence. *This band is always present.* It is the cheapest, the most reliable, and the only band with a fully trustworthy oracle. Deming's rule applies: build quality in here, do not inspect it in later.

4. **Add the output-type layer(s).**
   - code → tests (mutation-hardened) + SAST + bounded diff review (Ch. 3); don't-iterate-for-security.
   - factual → RAG grounding + claim-support / entailment (Ch. 4); citation exists ≠ citation correct.
   - reasoning → self-consistency / execution / proof checker / PRM (Ch. 5, Ch. 9); never ungrounded self-critique.
   - structured → semantic value validation *beyond* schema — ranges, enums, referential integrity, cross-field consistency (Ch. 6).
   - agentic → trajectory evaluation + checkpoint-before-irreversible (Ch. 7).

5. **Add an evaluator layer only where justified.** LLM-as-judge for *screening* (Ch. 8); PRM for step-level in *formal* domains (Ch. 9). **Never the sole high-stakes gate** — circularity and self-preference bias mean a model validating a model can launder, not catch, a class of error.

6. **Set the human gate by risk (Ch. 10).** None / spot-check / mandatory-with-forcing-function. Design *against* automation bias and approval fatigue: bounded scope, adversarial framing, risk-proportional triggering. The human is the last oracle and the most fallible; treat the slice as something to engineer, not something to rely on.

7. **Write the residual-risk statement (Ch. 12).** What no layer catches — correct-but-incomplete answers, sound-looking-but-wrong plans, novel-domain reasoning — *who owns it*, and the explicit **cannot-certify** line. This step is mandatory. A strategy missing it is incomplete.

Genichi Taguchi's robust-design insight is the lens on the whole flow: quality is the *loss* imparted by variation you couldn't remove, and you design a system to be *insensitive* to the noise you can't eliminate. The risk profile (step 2) is precisely Taguchi's tolerance dial — how much residual variation is *acceptable* is a design parameter set by stakes. James Reason's Swiss cheese (Ch. 11) is the picture of steps 3–6; Taguchi's loss function is the picture of step 2; and the open-bottomed residual box of step 7 is the honest admission both frameworks force — you manage variation toward target, you never reach zero.

> **Misconception to dislodge.** "A good validation strategy is the most layers I can afford." A good strategy is the layers the output type *needs* at the strictness the risk *demands*, deterministic-first, ending in a named residual. Over-validating low-risk output wastes the scarce human slice (Ch. 10) and breeds approval fatigue that then fails on the high-risk output. Strictness is a dial set by risk, not a slider you push to maximum.

---

## 13.3 Worked end-to-end: a customer-facing RAG support assistant

*Labeled hypothetical design exercise.* The system: a support assistant answering customer questions over a documentation corpus. Output types present: **factual claims** (dominant), **structured output** (citations, metadata), light **reasoning**. Risk profile: **medium-high** — wrong answers reach customers and carry reputational and occasional compliance exposure, but no single answer triggers an irreversible physical or financial action. We run the flow.

**Step 1–2: output types and risk.** Factual claims dominate, at medium-high risk; structured citations at medium (a malformed citation is annoying, not dangerous); light reasoning at medium. We design primarily for the factual layer and set compliance-sensitive categories (refunds, legal-adjacent, medical-adjacent) to *high* — those route differently in step 6.

**Step 3 — deterministic floor.** JSON-schema-validate the response envelope (answer + citation list + confidence). *Catches:* shape errors, missing-citation responses. *Misses:* everything semantic. *Plus* a **citation-existence check** — every cited document ID must resolve to a real chunk in the corpus; this is a lookup, deterministic, so it lives here. *Catches:* fabricated citations (the legal-brief failure mode, Ch. 4). *Misses:* a real citation that doesn't support the claim.

**Step 4 — factual layer (Ch. 4).** Claim-support / grounding: decompose the answer into claims, verify each is entailed by its cited span (entailment model or retrieval-overlap heuristic; Ragas-style metrics). *Catches:* unsupported claims, retrieval-error answers. *Misses:* subtle wrong paraphrase; correct-but-incomplete answers — and note this layer *shifts* hallucination into retrieval error rather than eliminating it.

**Step 5 — evaluator layer (Ch. 8).** An LLM-as-judge flags low-confidence or likely-unhelpful answers for human review — *screening only*. *Catches:* gross quality misses at scale. *Misses:* shared-failure-mode errors, where judge and generator are wrong the same way. We explicitly *do not* let the judge auto-approve high-stakes answers; that is the §11.1 failure we refuse to repeat.

**Step 6 — human gate by risk (Ch. 10).** Default answers: no human gate (the deterministic + grounding + screen layers carry them). Compliance-sensitive categories (the high-risk subset from step 2): mandatory human review with a cognitive-forcing checklist and bounded scope. *Catches:* high-stakes semantic errors. *Misses:* whatever the automation-biased reviewer waves through.

**Step 7 — residual-risk / cannot-certify statement.** *This pipeline cannot certify that a schema-valid, real-citation, entailment-passing, judge-screened answer is actually correct and complete in a category no human reviewed.* The uncaught classes are: (a) correct-but-incomplete answers (every layer passes, yet the answer omits a material caveat); (b) subtle paraphrase errors that the entailment heuristic scores as supported; (c) answers grounded in a *stale-but-real* document (the citation exists and even supports the claim, but the policy changed). None of these has a mechanical oracle — they are the factual-truth ceiling of Ch. 4 and the residual of Ch. 12. **Owner:** the product lead who sets the compliance-category list and the judge auto-approve threshold owns this residual; the honest move is to make that threshold a named, signed decision reviewed on a schedule, not a throughput knob.

### The same output type at two risk levels — the dial, made visible

To see risk *as* the dial, hold the output type fixed (factual claims) and change only the stakes.

| | **Low risk** (internal FAQ bot, advisory only) | **High risk** (clinical / financial advice surface) |
|---|---|---|
| Deterministic floor | schema + citation-existence | schema + citation-existence |
| Factual layer | citation-existence only | + full claim-support / entailment on every answer |
| Evaluator | LLM-judge screen | LLM-judge screen *plus* a second independent grounding check |
| Human gate | none | **mandatory** on every answer, forcing-function checklist |
| Residual statement | "may surface unsupported-but-plausible claims; users warned it's advisory" | "cannot certify factual correctness or completeness; a licensed human owns every answer; **defer to a professional**" |

Same five chapters, same output type. The *strictness* moved with the risk, and so did the residual statement — at high risk the cannot-certify line becomes a routing instruction to a human professional, because Ch. 12 says no layer closes that gap and the stakes will not tolerate the residual. That is the entire skill: read the layers off the type, set their strictness from the risk, and name what's left.

> **Misconception to dislodge.** "Once I've built the layers, the residual statement is boilerplate I can copy." The residual statement is the *most system-specific* part of the strategy and the hardest to write honestly, because it requires naming the failure your own layers can't catch — the one you are tempted to pretend away. A copied residual statement is a tell that the author didn't do the accounting. Write it last and write it specifically.

---

## 13.4 The box that never closes

Step back to the picture. Render the strategy top-to-bottom: a **mandatory deterministic floor** as the first band; **output-type layers** as a middle band that varies by type and risk; an **evaluator band** where justified; a **human gate** sized to risk; and at the bottom, the **residual-risk box** — drawn *open-bottomed* on purpose. The box never fully closes, and Ch. 12 is why: at the frontier, where the human is the oracle and is being outrun, there is no layer to draw beneath the human, so the box has no floor. Worse, the box *grows with capability* — as models produce output a human can verify less and less of, the uncaught region expands. A strategy correct today may under- or over-validate tomorrow as capability shifts the ground-truth-availability line; so the final discipline is to write the strategy to be **revisited**, on a named cadence, against a named signal (the observability bus's drift readout from Ch. 11).

This is the book's thesis in a single image. *Validation works where ground truth is mechanically available* — the deterministic floor and the grounded layers, the bands you can draw with confidence. *And fails where it isn't* — the open-bottomed box. Every honest validation strategy ends by drawing that box and refusing to pretend it has a floor. The engineer's job is not to close it — it cannot be closed — but to make it as small as the stakes require, to name what's inside it, and to assign it to a human who knows they own it.

> **Misconception to dislodge.** "A finished strategy has no residual risk." A finished strategy has a *named, owned, appropriately-sized* residual risk. Zero residual is not a goal you reached; it is a sign you stopped looking. The capstone competency — and the book's — is to design the pipeline *and state honestly what it cannot catch.* The honesty is the deliverable.

---

## Exercises

1. **(Apply / Analyze)** Take the §13.1 inventory ("We use Pydantic, a test suite, an LLM-judge, a human queue, SAST"). Convert it into an actual strategy for a *specific* output type of your choosing: index each named tool to the output type it defends, add a catch/miss line for each, drop the ones that don't apply, and add whatever the output type needs that the list omits. State explicitly what the original inventory left undefended.

2. **(Create, the graded capstone — produce-something)** Design the full validation strategy for the **agentic code pipeline** scenario (an autonomous coding agent that edits a repo, runs tools, opens PRs). Output types: generated code, agentic trajectory, structured tool calls. Run the seven-step flow. Produce: the per-type risk levels (justify why deploy/force-push forces high), the layer stack deterministic-first with the home-chapter citation for each layer, a catch/miss line per layer, and — mandatory — the residual-risk / cannot-certify statement with a named human owner. Mark your own work incomplete if it lacks step 7.

3. **(Evaluate)** Take any one output type and write its strategy at *low* and at *high* risk (as in §13.3's table). Then write one paragraph defending the *differences* as design decisions: what each added layer buys, what it costs (including human-attention cost and fatigue risk, Ch. 10), and why the residual statement changes between the two.

4. **(Evaluate)** A stakeholder reads your high-risk strategy and asks, "So with all these layers, can you guarantee the output is correct?" Write the honest answer in three sentences, grounded in Ch. 12: what the layers *do* certify, what they cannot, and who owns the gap. Then explain why answering "yes, it's validated" would be the single most dangerous sentence in the whole project.

---

## What would change my mind

The capstone's mandatory residual-risk statement rests on Ch. 12's verdict that some part of frontier output has no mechanical oracle. If scalable oversight were *solved* — if a validated method let a weaker overseer reliably judge a stronger system on open-ended output (the bar set in Ch. 12) — then the residual-risk box would acquire a floor, and the template's step 7 would change from "name what cannot be certified" to "apply the oversight method that certifies it." The whole capstone would simplify: the strategy would terminate in a layer rather than in an admission. As of this writing that method does not exist, so the open-bottomed box stands and the cannot-certify statement remains the one non-negotiable element. I would also revise the "risk is the only strictness dial" framing if it turned out that for some output type, *added layers reliably introduce more shared-failure-mode risk than they remove* (Ch. 8, Ch. 11) — in which case the dial would sometimes point *down*, toward fewer layers, even as risk rose.

## Still puzzling

- **Sizing the residual box.** There is no accepted method to *quantify* what a layered pipeline cannot catch for a given output type (Ch. 11, Ch. 12). The capstone teaches *naming* it, not measuring it, because the measurement doesn't reliably exist.
- **Diminishing returns and shared failure modes.** Stacking model-based layers (judge + PRM + agentic-judge) risks correlated errors (Ch. 8 circularity); the optimal stack composition for a given output type is uncharacterized.
- **The human gate under volume.** Automation bias and approval fatigue (Ch. 10) degrade exactly the layer the capstone leans on for residual risk; mitigations exist but no validated dosing.
- **Autonomy for agentic systems.** Checkpoint-before-irreversible is the defensible default (Ch. 7), but the precise boundary between auto-approve and human-gate for agentic actions is an open, risk-dependent design question.
- **Strategy drift.** A strategy correct today may mis-validate as capability shifts the ground-truth line (Ch. 12); the chapter teaches writing it to be revisited, but how often, and against what signal, is open.

---

## References

*This is a synthesis chapter; it introduces no new primary literature. Every citation traces to its home chapter, where it is verified. Inherited `[verify]` flags carry forward unchanged and are not re-asserted as confirmed here.*

- **Deterministic floor (Ch. 2):** compilers, typecheckers, schema validators (Pydantic, Zod, JSON Schema), linters, SAST.
- **Code (Ch. 3):** layered code stack; Veracode 2025 GenAI Code Security `[verify]`; iteration-degrades-security arXiv:2506.11022 `[verify]`.
- **Factual claims (Ch. 4):** Ayala & Bechard NAACL 2024; SemEval-2025 Mu-SHROOM; the legal-brief fabricated-cases failure.
- **Reasoning (Ch. 5):** self-consistency (Wang et al. 2022, arXiv:2203.11171) over self-critique (Huang et al. 2024, arXiv:2310.01798); execution/proof oracles.
- **Structured output (Ch. 6):** schema-then-semantic validation; constrained decoding (Willard & Louf 2023, arXiv:2307.09702).
- **Agentic execution (Ch. 7):** trajectory evaluation; checkpoint-before-irreversible; lab-to-prod gap arXiv:2511.14136 `[verify]`; AgentBench (Liu et al. 2023) `[verify]`.
- **LLM-as-judge (Ch. 8):** Zheng et al. 2024 (MT-Bench ~80%) `[verify]`; Shi et al. arXiv:2406.07791; the circularity problem.
- **PRMs (Ch. 9):** Lightman et al. 2023 (arXiv:2305.20050); ThinkPRM (arXiv:2504.16828); open-ended-task limit.
- **Human validation (Ch. 10):** cognitive forcing functions; automation-bias review (Lyell & Coiera 2017); approval fatigue (Buçinca et al. 2021; Beck et al. 2025).
- **Pipeline assembly (Ch. 11):** Reason's Swiss-cheese model; the failure-mode → catching-layer matrix; the observability bus.
- **Scalable oversight (Ch. 12):** Amodei et al. 2016 (arXiv:1606.06565); Bowman et al. 2022 (arXiv:2211.03540); Burns et al. 2023 (arXiv:2312.09390); Khan et al. 2024 (arXiv:2402.06782); Kenton et al. 2024 (arXiv:2407.04622); Stechly & Kambhampati 2023/2024. **The source of the mandatory residual-risk / cannot-certify statement.**
- **Design-discipline anchors:** Deming, *Out of the Crisis* (1986); Reason, *Human Error* (1990); Taguchi, robust design and the quality loss function.

---

**Tags:** capstone, validation-strategy, output-type-by-risk, deterministic-first, catch-miss-accounting, residual-risk, cannot-certify, decision-flow, rag-assistant, agentic-code-pipeline, strictness-dial, the-box-that-never-closes, deming-reason-taguchi
