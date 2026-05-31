# Chapter 13 — Capstone: Designing a Validation Strategy for a Real System

*Output type × risk profile in; layers, catch/miss, and an uncloseable residual-risk box out*

A team was asked to write the validation strategy for a new AI feature before launch. They produced a confident document: *We use Pydantic for schema validation. We have a test suite in CI. We run an LLM-as-judge for quality. We have a human review queue. We use SAST.* Every tool named was real. Every tool named was a tool this book endorses. The document was signed off, and the feature shipped.

It was not a strategy. It was an inventory.

Three things were missing. First, it was not indexed to **output type**. "We use SAST" is meaningful for generated code and irrelevant for factual claims. "We have a citation check" is meaningful for a RAG answer and meaningless for a tool call. A strategy reads its layers *off the output type*, and this list didn't say which tool defended which kind of output. Second, it had no **catch/miss accounting**. "We run an LLM-as-judge for quality" hides the question that matters — *what does the judge catch, and what does it miss?* A tool named without its holes is a tool you will over-trust. Third, and fatally, it had no **residual-risk statement**: no sentence naming what none of the layers catch, who owns that risk, and what the system therefore *cannot certify.* The document implied, by omission, that the listed tools covered everything. They did not — they never do (Chapter 12) — and the omission is the most dangerous thing in it.

A validation strategy is a *defensible argument*: for this output type, at this risk level, these layers in this order catch these failure classes; what remains uncaught is this, a named human owns it, and here is what we cannot certify. This chapter is the template for producing that argument, the worked example that fills it in, and the discipline of ending on the box that never closes.

<!-- → [FIGURE: Inventory vs. strategy contrast — left column labeled "Inventory": five bullet points (Pydantic, test suite, LLM-judge, human queue, SAST) with no output type, no catch/miss, no residual; right column labeled "Strategy": same five tools each indexed to an output type, each with a catch line and a miss line, ending in a residual-risk box; caption: "The inventory names tools. The strategy shows what each tool defends, what it misses, and what none of them catch."] -->

---

## The decision flow

The strategy is the output of a fixed procedure. Run it top to bottom for any system; it composes Chapters 2–12 into seven steps.

![A vertical seven-step flowchart: identify output types, set risk, lay the deterministic floor, add output-type layers, add an evaluator only where justified, set the human gate, and write the mandatory residual-risk statement. A branch off the risk step shows that an irreversible action forces high risk.](../images/13-capstone-designing-a-validation-strategy-fig-01.png)
![The seven-step validation-strategy decision flow, ending in the mandatory residual-risk statement.](images/13-capstone-designing-a-validation-strategy-fig-01.png)
*Figure 13.1 — The seven-step validation-strategy decision flow, ending in the mandatory residual-risk statement.*

**Step 1 — Identify the output type(s).** Code, factual, reasoning, structured, agentic. A real system usually produces more than one — a RAG product emits factual claims *and* structured citations; an agentic coder emits code *and* a trajectory *and* tool calls. Handle each type separately; they get different layers.

**Step 2 — Set the risk level per output type, from impact × reversibility.** Low, medium, high. The rule that overrides everything: **an irreversible action forces high** (Chapter 7 — checkpoint before destructive actions). A wrong answer a user can ignore is lower-risk than a wrong answer that triggers a refund, a deploy, or a data migration. Risk is not a vibe; it is impact times reversibility, written down.

**Step 3 — Lay the deterministic floor first (Chapter 2).** Which judgment-free checks apply? Compile, typecheck, schema-validate, lint, SAST, citation-existence. *This band is always present.* It is the cheapest, the most reliable, and the only band with a fully trustworthy oracle. Build quality in here; do not inspect it in later.

**Step 4 — Add the output-type layer(s).**
- Code → tests (mutation-hardened) + SAST + bounded diff review (Chapter 3); never iterate for security without re-scanning.
- Factual → RAG grounding + claim-support / entailment (Chapter 4); citation exists ≠ citation correct.
- Reasoning → self-consistency / execution / proof checker / PRM (Chapters 5, 9); never ungrounded self-critique.
- Structured → semantic value validation *beyond* schema — ranges, enums, referential integrity, cross-field consistency (Chapter 6).
- Agentic → trajectory evaluation + checkpoint-before-irreversible (Chapter 7).

**Step 5 — Add an evaluator layer only where justified.** LLM-as-judge for *screening* (Chapter 8); PRM for step-level in *formal* domains (Chapter 9). Never the sole high-stakes gate — circularity and self-preference bias mean a model validating a model can launder, not catch, a class of error.

**Step 6 — Set the human gate by risk (Chapter 10).** None, spot-check, or mandatory-with-forcing-function. Design against automation bias and approval fatigue: bounded scope, adversarial framing, risk-proportional triggering. The human is the last oracle and the most fallible; treat the slice as something to engineer, not something to rely on.

**Step 7 — Write the residual-risk statement (Chapter 12).** What no layer catches — correct-but-incomplete answers, sound-looking-but-wrong plans, novel-domain reasoning — *who owns it*, and the explicit **cannot-certify** line. This step is mandatory. A strategy missing it is incomplete.

<!-- → [INFOGRAPHIC: Seven-step decision flow as a vertical sequence — each step labeled with its chapter source; Step 7 shown with an open-bottomed box (no floor) to signal the residual never closes; arrows between steps show the deterministic floor as the widest band, narrowing toward the human gate, opening again at the residual] -->

Genichi Taguchi's robust-design insight is the lens on this whole procedure: quality is the *loss* imparted by variation you couldn't remove, and you design a system to be *insensitive* to the noise you can't eliminate. The risk profile (step 2) is Taguchi's tolerance dial — how much residual variation is *acceptable* is a design parameter set by stakes. You don't set it at maximum. You set it where the stakes demand.

---

## Worked end-to-end: a customer-facing RAG support assistant

The system: a support assistant answering customer questions over a documentation corpus. This is a labeled design exercise — not a description of any shipped product. Output types present: **factual claims** (dominant), **structured output** (citations, metadata), light **reasoning**. Risk profile: **medium-high** — wrong answers reach customers and carry reputational and occasional compliance exposure, but no single answer triggers an irreversible physical or financial action. Compliance-sensitive categories (refunds, legal-adjacent, medical-adjacent) are set to *high*.

![Four validation layers for a RAG assistant, each split into what it catches (left) and what it misses (right). Misses propagate down the right side from layer to layer and accumulate in a terminal open residual box holding three surviving uncaught classes with no mechanical oracle left.](../images/13-capstone-designing-a-validation-strategy-fig-04.png)
![Per-layer catch/miss accounting for the RAG assistant, with misses accumulating in the residual box.](images/13-capstone-designing-a-validation-strategy-fig-04.png)
*Figure 13.4 — Per-layer catch/miss accounting for the RAG assistant, with misses accumulating in the residual box.*

**Steps 1–2.** Factual claims dominate at medium-high risk; structured citations at medium (a malformed citation is annoying, not dangerous); light reasoning at medium. Compliance-sensitive categories escalate to high — they route differently in step 6.

**Step 3 — Deterministic floor.** JSON-schema-validate the response envelope. *Catches:* shape errors, missing-citation responses. *Misses:* everything semantic. Plus a **citation-existence check** — every cited document ID must resolve to a real chunk in the corpus; this is a lookup, not a judgment, so it belongs here. *Catches:* fabricated citations (the legal-brief failure mode from Chapter 4). *Misses:* a real citation that doesn't support the claim.

**Step 4 — Factual layer (Chapter 4).** Claim-support and grounding: decompose the answer into claims, verify each is entailed by its cited span (entailment model or retrieval-overlap heuristic; Ragas-style metrics). *Catches:* unsupported claims, retrieval-error answers. *Misses:* subtle wrong paraphrase; correct-but-incomplete answers. Note: this layer *shifts* hallucination into retrieval error rather than eliminating it.

**Step 5 — Evaluator layer (Chapter 8).** An LLM-as-judge flags low-confidence or likely-unhelpful answers for human review — *screening only.* *Catches:* gross quality misses at scale. *Misses:* shared-failure-mode errors where judge and generator are wrong the same way. The judge does not auto-approve high-stakes answers; that is the Chapter 11 failure we refuse to repeat.

**Step 6 — Human gate by risk (Chapter 10).** Default answers: no human gate (the deterministic, grounding, and screen layers carry them). Compliance-sensitive categories: mandatory human review with a cognitive-forcing checklist and bounded scope. *Catches:* high-stakes semantic errors. *Misses:* whatever the automation-biased reviewer waves through.

**Step 7 — Residual-risk / cannot-certify.** *This pipeline cannot certify that a schema-valid, real-citation, entailment-passing, judge-screened answer is actually correct and complete in a category no human reviewed.* The uncaught classes are: (a) correct-but-incomplete answers — every layer passes, yet the answer omits a material caveat; (b) subtle paraphrase errors that the entailment heuristic scores as supported; (c) answers grounded in a stale-but-real document — the citation exists and supports the claim, but the policy changed. None of these has a mechanical oracle. **Owner:** the product lead who sets the compliance-category list and the judge auto-approve threshold owns this residual. The honest move is to make that threshold a named, signed decision reviewed on a schedule, not a throughput knob.

<!-- → [TABLE: RAG support assistant layer stack — columns: Layer, Specific check, What it catches, What it misses; seven rows from schema to human gate; last row shows residual-risk / cannot-certify statement; Misses column shows the trajectory toward the open-bottomed box] -->

### The same output type at two risk levels

To see risk *as* the dial, hold the output type fixed (factual claims) and change only the stakes.

![Two side-by-side validation stacks for the same output type, factual claims, on a shared baseline. The low-risk stack carries a thin three-band stack with an advisory residual. The high-risk stack adds a second grounding check and a mandatory human gate, ending in a defer-to-a-professional residual.](../images/13-capstone-designing-a-validation-strategy-fig-03.png)
![The strictness dial: the same output type at two risk levels, the high-risk stack visibly taller.](images/13-capstone-designing-a-validation-strategy-fig-03.png)
*Figure 13.3 — The strictness dial: the same output type at two risk levels, the high-risk stack visibly taller.*

| | **Low risk** — internal FAQ bot, advisory only | **High risk** — clinical or financial advice |
|---|---|---|
| Deterministic floor | schema + citation-existence | schema + citation-existence |
| Factual layer | citation-existence only | full claim-support / entailment on every answer |
| Evaluator | LLM-judge screen | LLM-judge screen + second independent grounding check |
| Human gate | none | **mandatory** on every answer, forcing-function checklist |
| Residual statement | "may surface unsupported-but-plausible claims; users warned advisory" | "cannot certify correctness or completeness; a licensed human owns every answer; **defer to a professional**" |

Same five chapters. Same output type. The strictness moved with the risk, and so did the residual statement — at high risk, the cannot-certify line becomes a routing instruction to a human professional, because Chapter 12 says no layer closes that gap and the stakes will not tolerate the residual. That is the entire skill: read the layers off the type, set their strictness from the risk, and name what's left.

---

## The box that never closes

Step back to the picture. A **mandatory deterministic floor** as the first band. **Output-type layers** as a middle band that varies by type and risk. An **evaluator band** where justified. A **human gate** sized to risk. And at the bottom, the **residual-risk box** — drawn *open-bottomed* on purpose.

![A validation strategy as a stack of bands: a solid deterministic floor, output-type layers, an inset evaluator band where justified, and a human gate sized to risk. Beneath sits the residual-risk box drawn open-bottomed on purpose, with no floor edge, because at the frontier the box never fully closes.](../images/13-capstone-designing-a-validation-strategy-fig-02.png)
![The layered strategy over the residual-risk box drawn open-bottomed because it never closes.](images/13-capstone-designing-a-validation-strategy-fig-02.png)
*Figure 13.2 — The layered strategy over the residual-risk box drawn open-bottomed because it never closes.*

The box never fully closes, and Chapter 12 is why: at the frontier, where the human is the oracle and is being outrun, there is no layer to draw beneath the human, so the box has no floor. The box also *grows with capability* — as models produce output a human can verify less and less of, the uncaught region expands. A strategy correct today may under- or over-validate tomorrow as capability shifts the ground-truth-availability line. So the final discipline is to write the strategy to be *revisited*, on a named cadence, against a named signal: the observability bus's drift readout from Chapter 11.

<!-- → [FIGURE: Full validation stack as a visual — bands labeled from top: Deterministic Floor (widest, solid), Output-Type Layers, Evaluator Band (narrower), Human Gate (thinnest); below all bands, an open-bottomed box labeled "Residual Risk / Cannot Certify" with an arrow labeled "grows with capability"; caption: "The stack is drawn top-down by ground-truth availability. The box at the bottom has no floor — that is the honest admission both the Swiss-cheese model and Taguchi's loss function force."] -->

This is the book's thesis in a single image. *Validation works where ground truth is mechanically available* — the deterministic floor and the grounded layers, the bands you can draw with confidence. *And fails where it isn't* — the open-bottomed box. Every honest validation strategy ends by drawing that box and refusing to pretend it has a floor. The engineer's job is not to close it — it cannot be closed — but to make it as small as the stakes require, to name what's inside it, and to assign it to a human who knows they own it.

A finished strategy does not have zero residual risk. It has a *named, owned, appropriately-sized* residual risk. Zero residual is not a goal you reached; it is a sign you stopped looking. The capstone competency — and the book's — is to design the pipeline *and state honestly what it cannot catch.* The honesty is the deliverable.

---

## LLM Exercises

**Exercise 13.1 (Apply/Analyze).** Take the §13.1 inventory ("We use Pydantic, a test suite, an LLM-judge, a human queue, SAST"). Convert it into an actual strategy for a *specific* output type of your choosing: index each named tool to the output type it defends, add a catch/miss line for each, drop the tools that don't apply, and add whatever the output type needs that the list omits. State explicitly what the original inventory left undefended.

**Exercise 13.2 (Create — graded capstone).** Design the full validation strategy for an **agentic code pipeline** (an autonomous coding agent that edits a repo, runs tools, opens PRs). Output types: generated code, agentic trajectory, structured tool calls. Run the seven-step flow. Produce: per-type risk levels (justify why deploy/force-push forces high), the layer stack deterministic-first with the home-chapter citation for each layer, a catch/miss line per layer, and — mandatory — the residual-risk / cannot-certify statement with a named human owner. Mark your own work incomplete if it lacks step 7.

**Exercise 13.3 (Evaluate).** Take any one output type and write its strategy at *low* and at *high* risk (as in the §13.3 table). Then write one paragraph defending the *differences* as design decisions: what each added layer buys, what it costs (including human-attention cost and fatigue risk, Chapter 10), and why the residual statement changes between the two.

**Exercise 13.4 (Evaluate).** A stakeholder reads your high-risk strategy and asks: "So with all these layers, can you guarantee the output is correct?" Write the honest answer in three sentences, grounded in Chapter 12: what the layers *do* certify, what they cannot, and who owns the gap. Then explain why answering "yes, it's validated" would be the single most dangerous sentence in the whole project.
