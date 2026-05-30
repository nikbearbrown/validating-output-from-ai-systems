> **Voice status:** `voice-unanchored`. Style folder empty as of this draft.

---

# Chapter 2 — The Deterministic Floor: Validators That Don't Need Judgment

*Find the Compiler-Equivalent for Every Output Type, and Make It Block the Merge*

**Author:** Nik Bear Brown
**Editor:** Nik Bear Brown

---

## Suggested titles

1. **The Deterministic Floor: Validators That Don't Need Judgment — Find the Compiler-Equivalent for Every Output Type, and Make It Block the Merge**
2. **Same Input, Same Verdict, Every Time: Why the Cheapest External Check Beats the Model's Best Opinion**
3. **What's the Compiler-Equivalent Here? Building the Judgment-Free First Layer of Any Validation Pipeline**

---

## TL;DR

Chapter 1 showed you cannot trust the generator to validate itself and you cannot make a fluent-defeated human the first line of defense. The constructive answer starts with the layer where neither is needed: **deterministic validators** — tools that return the *same verdict for the same input, every time, with no judgment.* A compiler rejects ill-typed code; a JSON-Schema validator rejects a non-conforming document; a linter flags its rule violations; SAST flags rule-matched vulnerability patterns; a secrets scanner finds committed credentials by pattern. These are *exact within their domain*, immune to fluency (the FSM does not care how confident the model sounds), and cheap enough to run on every change in CI where they **block the merge.** Two converging bodies of evidence justify making this the *first* layer: the negative result that intrinsic self-correction is unreliable (Huang et al. 2024), and the positive result that *execution feedback* — a deterministic signal — materially improves code generation where ungrounded self-critique does not (Chen et al. 2024, "Teaching LLMs to Self-Debug"; reinforced at training scale by RLEF, Gehring et al. 2024). The transferable skill is not a tool list; it is a *move*: for any output type, ask **"what property can be checked deterministically, and what tool returns that verdict?"** — shape → schema, build → compiler, pattern → SAST. Constrained decoding is the purest case: compile a schema into a finite-state machine that gates token generation and structurally-invalid output becomes *unrepresentable* — a mathematical guarantee of shape. But the floor has a hard ceiling, and you must internalize it before you over-trust it: **it guarantees *form*, not *intent*.** Code can compile, pass the linter, pass SAST, and still do the wrong thing; JSON can validate and carry wrong values. The deterministic floor is *necessary, cheap, and non-negotiable* — and *not sufficient.* That gap is why Chapters 3–7 layer tests, retrieval grounding, judges, PRMs, and humans on top, output type by output type.

---

## Learning objectives

By the end of this chapter you should be able to:

1. **(Understand)** Define a *deterministic validator* and explain why same-input-same-verdict makes it immune to the fluency problem of Chapter 1.
2. **(Understand)** Explain why external deterministic feedback beats model self-judgment, citing the negative result on self-correction and the positive results from execution-grounded loops.
3. **(Apply)** Given an AI output type, identify the deterministic property that can be checked and name the tool that returns that verdict — the "what's the compiler-equivalent here?" move.
4. **(Apply — produce something)** Wire a minimal CI gate (typecheck + lint + schema-validate) that blocks a fluent-but-broken AI output, and demonstrate it rejecting a valid-shape-wrong-value example.
5. **(Evaluate)** Judge where to place a CI gate under SAST false-positive load — what severity should *fail* the build versus merely *flag* — and defend the tradeoff between false positives and false negatives.
6. **(Analyze)** Demonstrate the floor's ceiling: produce an output that passes every deterministic check and is still wrong, and explain why no amount of deterministic checking would have caught it.

## Prerequisites

Chapter 1 — the generate-verify asymmetry's failure for LLMs, the fluency problem, and the validation-responsibility stack (you are now building out layers 2 and 3). Working familiarity with at least one compiled or typed language, a CI system (GitHub Actions, GitLab CI, or similar), and the idea of a schema (JSON Schema, Pydantic, or Zod). No formal-methods background required; the Floyd/Hoare lineage from Chapter 1 is the only theory invoked.

---

## 2.1 The output you would never ship — and the one you would

Ask yourself a question that exposes a double standard you already hold. Would you ship code that does not compile? Of course not. You would not even *consider* it. The compiler is a gate you have never once argued with: red means stop, and you fix it before the thought of merging crosses your mind. You do not call a meeting about whether the type error is *really* an error. You do not weigh the compiler's confidence against your own. It returns a verdict and the verdict is final, because it is *exact*.

Now: would you ship an AI-generated answer that "reads right"? You probably have, this week. The clean diff from Chapter 1 §1.1 got approved in two minutes. The factual paragraph with confident citations went into the report. The structured-data blob that *looked* like valid JSON got passed downstream. For these you *did* hold a meeting in your head — *does this look okay?* — and you let fluency cast the deciding vote.

Here is the whole chapter in that asymmetry. You already own a judgment-free gate for one output type. You trust it completely, precisely *because* it does not have an opinion — it has a verdict. The deterministic floor is the discipline of extending that instinct to *every* AI output type: stop asking "does this look right?" and start asking **"what's the compiler-equivalent here?"** For code, the literal compiler. For structured data, the schema validator. For security patterns, SAST. For committed secrets, a pattern scanner. Each is a tool that returns the same verdict for the same input, every time, *without judgment* — and so each is immune to exactly the thing that defeated you in Chapter 1: it cannot be fooled by fluency, because it never reads the output the way you do. It checks a property, not an impression.

This is the cheapest, most trustworthy layer in the stack, and it is therefore where you start. Not because it is the most powerful — it is the *least* powerful, as §2.5 will insist — but because it is the only layer whose verdict is free of both the model's self-delusion and your strained attention. You build up from a floor you can stand on.

---

## 2.2 What "deterministic" buys you, and why it beats the model's opinion

A **deterministic validator** is a function from output to verdict that is *referentially transparent*: same input, same verdict, every time, with no judgment and no hidden state. Run the typechecker on the same source twice and you get the same answer. Validate the same document against the same JSON Schema on two machines, two years apart, and the verdict is identical. There is no temperature, no sampling, no "it depends how you ask." This is in the deepest sense the Floyd/Hoare inheritance from Chapter 1: correctness-of-*a-checkable-property* is turned into a mechanical question with a mechanical answer.

Three properties follow, and each maps directly onto a Chapter 1 failure it defeats:

1. **Exactness within its domain.** A compiler rejects ill-typed code; a schema validator rejects a non-conforming document; a linter flags its rule violations — no false uncertainty, no "I think this is probably fine." Within the property it checks, it is *right.* (Outside that property it is silent — §2.5.)
2. **Immunity to fluency.** The validator never reads the output as prose. It does not perceive confidence, clean structure, or a convincing docstring. The finite-state machine in §2.4 *does not care how sure the model sounds.* This is the direct antidote to §1.3: the surface features that defeat a human reviewer are invisible to a deterministic check.
3. **Repeatability, hence automatability.** Because the verdict is exact and stateless, you can run it on every change, automatically, in CI, and make it *block the merge.* No human is in the critical path for the properties it covers. This is the direct antidote to §1.5: it moves the load-bearing check off the expensive, bias-prone human layer and onto cheap infrastructure.

Now, why make this the *first* layer rather than asking the model to check itself, which would be even cheaper to invoke? Because Chapter 1 already answered it and the evidence converges from two directions.

**The negative result.** Huang et al. (2024) showed intrinsic self-correction — re-examination with no external signal — does not dependably improve answers and can degrade them. The model has no independent ground truth, so "check your work" is just another generation. If the validation signal is going to be trustworthy, it must come from *outside* the model. The deterministic validator is the cheapest possible "outside": a tool whose verdict does not depend on the model's opinion at all.

**The positive result.** Chen et al. (2024), "Teaching Large Language Models to Self-Debug," showed that when you feed a model *execution feedback* — run the code, return the actual error and trace — code-generation accuracy improves materially, in sharp contrast to ungrounded self-critique. The mechanism is exactly the chapter's point: a deterministic tool (the interpreter, the compiler) converts a fuzzy "is this right?" into a *hard signal* — the program crashed at line 12 with this stack trace — that the model can actually act on. The interpreter, not the model's self-assessment, is the oracle. Gehring et al. (2024, RLEF) push the same principle up into the *training* loop: grounding generation in execution results drives reliability at scale. So the deterministic-feedback principle is not an inference-time hack bolted on at the end; it is the load-bearing signal from runtime loops all the way up to how the models are trained.

> **Misconception to dislodge.** "External" must mean a second model. It does not. The most reliable external signal is the *least* intelligent one: a tool that mechanically checks a property and cannot be argued with. A compiler is a better validator of compilability than the smartest model's opinion of compilability, because the compiler *is* the ground truth for that property and the model only *predicts* it. When a deterministic check exists for a property, use it; do not spend a model call predicting what a tool can decide.

I will be precise about the evidence and not overclaim a number. The self-debug accuracy gains are real *in direction and mechanism* — deterministic feedback helps, ungrounded self-critique does not reliably — but the exact deltas are benchmark- and model-specific, so this chapter cites the mechanism, not a single headline percentage. `[verify — present execution-feedback improvements as direction-and-mechanism, not a fixed delta; the magnitude is benchmark/model-dependent.]`

---

## 2.3 The move: find the floor for any output type

The skill this chapter wants to leave in your hands is *not* a memorized tool list — tool lists rot, and a list teaches you nothing about the output type the list forgot. The transferable skill is a question you run on *any* AI output: **"What property of this output can be checked deterministically, and what tool returns that verdict?"** Three quick patterns to calibrate the move:

- **Shape → schema.** Is the output supposed to have a structure? Then a schema validator decides conformance exactly. JSON Schema is a deterministic contract: a document validates or it does not, with a machine-checkable verdict and no judgment. Pydantic (Python) and Zod (TypeScript) compile such contracts into runtime validators you can call before any downstream code touches the data.
- **Build → compiler/typechecker.** Is the output code in a typed language? Then "does it build / are the types sound?" is decided by the compiler, exactly. Niklaus Wirth's argument that strict static typing is a *feature*, not a constraint, is precisely this: the typechecker is a judgment-free validator of program structure that catches whole classes of error before the code ever runs.
- **Pattern → static analysis.** Is there a class of defect expressible as a *pattern* in the source? Then SAST and linters decide rule-matches exactly. This is the lineage of Frances Allen's static program analysis — reasoning about a program's properties from its source *without executing it*. A linter flagging an unused variable, a SAST rule flagging a string-concatenated SQL query, a secrets scanner flagging a committed API key: each is a deterministic pattern check that runs in CI and blocks the merge.

Run the move across the output types this book will cover and the floor appears for each:

| Output type | Deterministic property | Tool that returns the verdict |
|---|---|---|
| Generated code | Compiles? Types sound? | Compiler / typechecker |
| Generated code | Rule-matched defect/style? | Linter, SAST |
| Generated code | Committed secret? | Secrets scanner (gitleaks/trufflehog-class) |
| Structured data / tool calls | Conforms to schema? | JSON Schema / Pydantic / Zod |
| Config / IaC | Valid config? Policy-compliant? | Schema validation + policy-as-code (OPA-style) |
| Factual claims | *Does the cited source exist?* | Citation-existence lookup (deterministic at the existence layer; Ch. 4) |

The last row is the instructive one. Even for *factual* output — which seems hopelessly judgment-laden — there is a deterministic *sub-property*: a citation either resolves to a real document or it does not. That check would have caught the fabricated-case legal briefs of Chapter 1 §1.3 *without any judgment about the argument's quality.* Finding that decidable sub-property inside an apparently-undecidable output type is the move at its most valuable. You will rarely get the *whole* output type onto the floor — but you can almost always get *some property* of it there, and that property is then checked for free, exactly, forever.

---

## 2.4 The purest case: constrained decoding makes invalid shape unrepresentable

There is one deterministic validator so strong it deserves its own section, because it changes *when* the check happens. Ordinarily you generate first and validate after: the model emits a JSON blob, then you run the schema validator and accept or reject. **Constrained decoding** moves the check *into generation.* Compile the schema (or a grammar) into a finite-state machine, and at each decoding step mask out any token that would make the partial output unable to complete into a valid document. The model is only ever *allowed* to emit tokens consistent with the schema.

The consequence is categorical: structurally-invalid output becomes *unrepresentable.* Not "unlikely," not "rare with a good prompt" — impossible. The FSM does not permit the tokens that would form an invalid document. This is the cleanest illustration in the whole book of a validator that *cannot be fooled by fluency*: the machine does not read the output, does not perceive the model's confidence, does not weigh how convincing the surrounding prose is. It enforces a structural constraint at the token level, mechanically. You get a **mathematical guarantee of shape**, not a statistical one — a different *kind* of assurance than "we prompted it nicely and it usually returns valid JSON."

This is also the right place to retire a habit. "Please return only valid JSON" in a prompt is a *hope*, and Chapter 1's brittleness cousins will tell you hopes are fragile. A provider-level structured-output mode backed by constrained decoding is a *contract.* The shift from prompt-hope to API-guarantee is one of the genuine recent improvements in this space: structured-output / JSON-mode with constrained decoding replaced "please return JSON" prompting across mainstream providers, turning a probabilistic ask into a deterministic shape contract. When the property you need is shape, prefer the API-level guarantee to the prompt-level plea.

> **Misconception to dislodge.** "Constrained decoding (or schema validation) guarantees the output is correct." It guarantees the output is *well-shaped.* This is exactly the Chapter 1 trap wearing a new costume: well-formed is not correct. A schema that requires `{"temperature_celsius": number}` is perfectly satisfied by `{"temperature_celsius": 9999}`. The FSM guarantees you got a number in the right slot; it has *no opinion* about whether 9999 is a sane temperature. Constrained decoding moves you from "might not be parseable" to "parseable and structurally valid" — a real and valuable move — and *not one inch* toward "the values are right." That is the floor's ceiling, and the next section makes you stare at it.

Two honest caveats, because the book shows its uncertainty. There is an open question about whether forcing the FSM *degrades content quality* while guaranteeing shape — the evidence is mixed and model-dependent, so treat constrained decoding as a shape guarantee with a possible (measurable) quality cost, not a free lunch. `[verify — constrained-decoding quality/cost tradeoff is contested and model-specific.]`

---

## 2.5 The ceiling: form is not intent

Everything above is the floor's strength. This section is its limit, and you must hold it as firmly as the strength, because the characteristic failure of teams that *discover* the deterministic floor is to *over-trust* it — to mistake a green CI pipeline for a correct system.

State it plainly: **the deterministic floor catches *form* and *rule-matched patterns*, and says nothing about *intent* or *semantics.***

- **Code** can compile, satisfy the typechecker, pass every linter rule, and clear SAST — and implement the *wrong specification.* The off-by-one date bug from Chapter 1 §1.1 would sail through the entire deterministic floor. It is well-typed. It is well-styled. It contains no rule-matched vulnerability. It is also wrong, and no compiler on earth knows the intended semantics of "billing period" to tell you so.
- **Structured data** can validate against the schema and carry *wrong values.* The temperature of 9999°C validates. A `currency: "USD"` field validates whether the amount is right or off by a factor of a hundred. Shape is decided; *truth* is not.
- **SAST** is deterministic in its *rule check* but its *coverage* is a judgment call, and it trades false positives against false negatives. Tighten the rules to catch more real vulnerabilities and you raise the false-positive load that erodes the team's trust in the gate; loosen them to cut false positives and you raise the silent false negatives. Industry-good tools are sometimes quoted around ~5% false-positive rates, and a ~1M-LOC codebase might surface on the order of ~20,000 findings with ~1,000 false positives — but **these are vendor/industry illustrative figures, not peer-reviewed constants**, and I cite them only to make the *tradeoff* concrete. `[verify — SAST scale/noise numbers (~20k findings, ~1k FP per ~1M LOC, ~5% FP) are vendor illustrative, not measured constants.]` The determinism lives in the *rule check*; the *rule set's* coverage and tuning are human decisions, and so is the policy of what severity *fails* the build versus merely *flags* it.

So the deterministic floor is **necessary, cheap, non-negotiable in CI — and not sufficient.** It is the part of the validation problem where ground truth is mechanically available *by construction*: well-formedness (compiler), structural conformance (schema), rule-matched patterns (linter/SAST/secrets). For those properties the verdict is exact and neither the model's self-assessment nor the human's bias-prone attention is the load-bearing check — which is the entire reason to start here. But "by construction" is also the limit: the floor only covers the properties you can *construct* a mechanical check for, and *intent* is not one of them.

This is precisely why the book does not stop at Chapter 2. Above the floor, ground truth stops being mechanically free, and you have to *earn* it — tests that encode the spec (Chapter 3), retrieval grounding and citation-support checking (Chapter 4), reasoning verification (Chapter 5), semantic validation of structured values (Chapter 6), trajectory checks for agents (Chapter 7), and then the judges and humans of Act Three. Each layer is more powerful and more expensive and *less* certain than the floor. The discipline is to push every check as far *down* toward the floor as it will go — get every decidable property checked for free — and to know exactly which residual properties cannot live there, so you spend your expensive layers only on what genuinely needs them.

> **Misconception to dislodge.** "Green CI means the output is correct." Green CI means the output passed every property your deterministic floor can decide. It is a statement about *form*, not *intent.* The clean diff that destroys data, the valid JSON with the catastrophic value, the well-typed code that implements last quarter's spec — all are green. Treat the floor as the *entry condition* for the layers above it, never as the verdict. Chapter 3 begins where the floor ends.

---

## Exercises

1. **(Apply — produce something)** Wire a minimal CI gate for an AI-generated output. Pick a small typed project (or a JSON-producing endpoint). Configure a pipeline (GitHub Actions / GitLab CI / a Makefile target) that runs, in order: a typecheck (or schema validation), a linter, and — for structured output — a JSON-Schema/Pydantic/Zod validation. Then *deliberately* feed it a fluent-but-broken AI output and demonstrate the gate rejecting it. Deliverable: the pipeline config plus a screenshot/log of the gate going red, and one sentence naming exactly which deterministic property failed.

2. **(Analyze — demonstrate the ceiling)** Produce an AI output that passes *every* check in your Exercise-1 gate and is still wrong. For code: a well-typed, lint-clean function implementing the wrong spec. For structured data: a schema-valid document with a semantically wrong value. Deliverable: the passing-but-wrong artifact, the green pipeline output, and a paragraph explaining *why no deterministic check in your gate could have caught it* — tie this explicitly to §2.5's "form is not intent."

3. **(Evaluate)** You run SAST on a 500k-LOC codebase and get 9,000 findings; spot-checking suggests roughly 8% are false positives. Leadership wants the build to *fail* on findings so they get fixed. (a) Propose a gating policy: which severities should *fail* the build, which should merely *flag*, and why. (b) Quantify the cost of your policy — roughly how many false positives will block builds per week given your assumptions, and what that does to team trust in the gate. (c) Defend your line against the opposite policy ("fail on everything") using the false-positive/false-negative tradeoff from §2.5. (d) State explicitly which part of your decision is *deterministic* (the rule check) and which part is *judgment* (the gating policy).

4. **(Apply)** For each of these AI output types, run the §2.3 move and name (i) one deterministic property you can check and (ii) the tool that returns the verdict, then (iii) one property that is *not* on the floor: an SQL migration script; a citation-bearing factual summary; a Kubernetes manifest generated by an agent; a function that parses user-uploaded CSVs. Deliverable: a four-row table with all three columns filled. The third column is the point — it is the handoff to later chapters.

---

## What would change my mind

The chapter's claim is that deterministic validators belong *first* because they are the only layer whose verdict is exact, fluency-immune, and free of the model's self-judgment. A finding that would force revision: a robust, replicated demonstration that a *non-deterministic* external check — say, a well-calibrated learned verifier or an LLM judge with measured-and-stable error rates — reliably catches the *same* decidable properties (compilability, schema-conformance, known vulnerability patterns) *more cheaply or more completely* than the deterministic tool across realistic codebases, *and* that its verdicts are stable enough to gate a merge on. If that held, "deterministic first" would soften to "exact-and-cheap first, which is usually but not always the deterministic tool." I think this is unlikely for the *decidable* properties — the compiler simply *is* the ground truth for compilability, so a predictor cannot beat it on that property — but it is exactly the kind of claim I would want settled by measurement rather than asserted, and the boundary between "decidable property" and "property a learned checker handles better" may move as the learned checkers improve.

## Still puzzling

- **How far up the output-type ladder can deterministic checks reach** before judgment becomes unavoidable? Semantic schema constraints (value ranges, cross-field invariants) and property-based tests are a genuine bridge — partly deterministic, partly spec-dependent — and where exactly the floor ends is not crisp.
- **Optimal CI gating policy under SAST false-positive load** — what severity should block versus flag — lacks a principled, evidence-based standard. It is mostly folklore and vendor defaults, and the right answer surely depends on the team's tolerance and the cost of a missed defect, neither of which the tooling measures for you.
- **Constrained-decoding cost/quality tradeoffs.** Does forcing the FSM degrade *content* quality while guaranteeing shape? The evidence is mixed and model-dependent, so "always use constrained decoding for structured output" is not yet a settled recommendation.
- **Coverage of secrets/SAST rule sets against *AI-introduced* vulnerability classes.** Code models may produce novel insecure patterns that predate no rule. How well today's deterministic rule sets cover tomorrow's AI-generated defect distribution is an open, fast-moving question — and a reminder that the floor's *coverage* (not its determinism) is where it ages.

---

## References

- Huang, J., Chen, X., Mishra, S., Zheng, H. S., Yu, A. W., Song, X., & Zhou, D. (2024). [Large Language Models Cannot Self-Correct Reasoning Yet](https://arxiv.org/abs/2310.01798). *ICLR 2024.* arXiv:2310.01798.
- Chen, X., Lin, M., Schärli, N., & Zhou, D. (2024). [Teaching Large Language Models to Self-Debug](https://arxiv.org/abs/2304.05128). *ICLR 2024.* arXiv:2304.05128. `[verify arXiv ID before quoting]`
- Gehring, J., et al. (2024). [RLEF: Grounding Code LLMs in Execution Feedback with Reinforcement Learning](https://arxiv.org/abs/2410.02089). arXiv:2410.02089.
- Floyd, R. W. (1967). Assigning Meanings to Programs. *Proceedings of Symposia in Applied Mathematics*, Vol. 19, American Mathematical Society, 19–32.
- Hoare, C. A. R. (1969). An Axiomatic Basis for Computer Programming. *Communications of the ACM* 12(10), 576–580. DOI:10.1145/363235.363259.
- JSON Schema. [JSON Schema Specification](https://json-schema.org/specification). (Stable tooling reference.)
- Pydantic. [Pydantic documentation](https://docs.pydantic.dev/). (Python runtime validation.)
- Zod. [Zod documentation](https://zod.dev/). (TypeScript runtime validation.)
- OWASP. [Source Code Analysis Tools (SAST)](https://owasp.org/www-community/Source_Code_Analysis_Tools). (Reference for the deterministic security layer.)
- SAST scale/noise figures (~20k findings, ~1k FP per ~1M LOC; ~5% FP for best-in-class tools): vendor/industry illustrative figures (CrowdStrike, GitLab, Sonar, Checkmarx vendor docs). `[verify — order-of-magnitude illustrative, NOT peer-reviewed constants.]`

---

**Tags:** deterministic-validators, deterministic-floor, compiler-equivalent, schema-validation, constrained-decoding, structured-output, sast, linting, secrets-scanning, execution-feedback, external-feedback-beats-self-judgment, form-not-intent, ci-gating, false-positive-false-negative
