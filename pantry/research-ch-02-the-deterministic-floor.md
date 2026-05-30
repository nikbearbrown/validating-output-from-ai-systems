# Research: Chapter 02 — The Deterministic Floor: Validators That Don't Need Judgment
## Validating Output from AI Systems
**Chapter one-line:** Compilers, typecheckers, schema validators, linters, and SAST as the first cheap, unambiguous validation layer — the "deterministic validators first" principle, and why external + deterministic feedback beats model self-judgment.
**Research date:** 2026-05-29

---

## 1. Primary Sources

### Foundational papers and texts

- **Huang, Jie; et al. "Large Language Models Cannot Self-Correct Reasoning Yet." ICLR 2024 (arXiv:2310.01798).** The negative result that *justifies* the deterministic floor: because intrinsic self-correction is unreliable, the validation signal must come from *outside* the model. This chapter is the constructive answer to that paper — the cheapest external signal is a deterministic tool that returns the same verdict every time. Cite to motivate "why not just ask the model to check?"

- **Chen, Xinyun; Lin, Maxwell; Schärli, Nathanael; Zhou, Denny. "Teaching Large Language Models to Self-Debug." ICLR 2024.** Shows that when an LLM is given *execution feedback* (run the code, feed back the error/trace), code-generation accuracy improves materially — in contrast to ungrounded self-critique. The mechanism the chapter teaches: a deterministic tool (the interpreter/compiler) converts a fuzzy "is this right?" into a hard signal the model (or pipeline) can act on. The interpreter, not the model's opinion, is the oracle.

- **Gehring, Jonas; et al. "RLEF: Grounding Code LLMs in Execution Feedback with Reinforcement Learning." (arXiv:2410.02089).** Reinforces the same point at training scale: grounding generation in *execution results* (a deterministic signal) drives reliability. Useful as evidence that the deterministic-feedback principle holds from inference-time loops up to model training — the floor is not a hack, it's the load-bearing signal.

- **JSON Schema specification + Pydantic / Zod (tooling references).** The structural-validation primitives. JSON Schema is a deterministic contract: a document either validates or it doesn't, with a machine-checkable verdict and no judgment. Pydantic (Python) and Zod (TypeScript) compile such contracts into runtime validators. Key chapter point on **constrained decoding / structured-output APIs:** when a schema is compiled into a finite-state machine that gates token generation, the model *cannot* emit a structurally invalid document — a *mathematical* guarantee of shape, not a statistical one. Crucial nuance: this guarantees *shape*, not *semantics* (valid JSON with wrong values still validates) — set up for Ch. 6.

- **Static analysis / SAST tooling (CrowdStrike, GitLab, Sonar, Checkmarx vendor docs; OWASP).** The deterministic security/quality layer. SAST analyzes source without running it and flags rule-matched patterns; linters enforce style/correctness rules; secrets scanners (e.g., gitleaks/trufflehog-class tools) detect committed credentials by deterministic pattern. Chapter point: these run automatically in CI and *block the merge* — judgment-free gates. Honest limit to teach: SAST trades false positives against false negatives (industry-good tools ~5% FP; tightening rules to cut FPs raises FNs), so deterministic ≠ complete.

### Key empirical cases

- **Constrained decoding gives a 100%-valid-shape guarantee.** Documented and reproducible: schema-compiled-to-FSM (or grammar-constrained) decoding makes structurally-invalid output unrepresentable at generation time. Cite as the cleanest example of a deterministic validator that *cannot* be fooled by fluency — the FSM does not care how confident the model sounds.

- **SAST scale-and-noise numbers.** Industry-reported: a ~1M-LOC codebase may surface on the order of ~20,000 SAST findings with ~1,000 false positives even with good tools (vendor-reported figures). **[verify — these are vendor/industry figures, order-of-magnitude illustrative, not a peer-reviewed measurement; present as illustrative of the FP/FN tradeoff, not a precise constant.]**

- **Execution-feedback self-debug improvement.** Chen et al. (ICLR 2024) report accuracy gains on code benchmarks when execution feedback is supplied versus ungrounded re-prompting. Documented; use the *direction and mechanism* (deterministic feedback helps; ungrounded self-critique does not reliably) rather than a single headline percentage, since exact deltas are benchmark- and model-specific.

---

## 2. The Core Concept — State of the Field

### What is settled
- **Deterministic validators are exact within their domain.** A compiler rejects ill-typed code; a JSON-Schema validator rejects a non-conforming document; a linter flags its rule violations — same input, same verdict, every time, no judgment. This is not contested.
- **External, deterministic feedback beats model self-judgment.** Converging from two directions: the negative result on intrinsic self-correction (Huang et al.) and the positive results from execution-grounded loops/training (Chen et al.; RLEF). The reliable lever is the *external* deterministic signal.
- **Constrained decoding guarantees structure.** Grammar/FSM-constrained generation makes invalid *shape* impossible — a genuine guarantee, widely deployed in production structured-output APIs.

### What is disputed
- **How much of the validation burden the deterministic floor can carry.** It catches *form* and *rule-matched patterns* with certainty, but says nothing about *intent/semantics*: code can compile and pass the linter and still do the wrong thing; JSON can validate and carry wrong values. The dispute is about where to draw the line and how much downstream (test/judge/human) work remains — a recurring book theme.
- **SAST signal-to-noise and where to gate the build.** Practitioners disagree on what severity should *fail* CI vs. merely flag, because false positives erode trust and false negatives are silent. The determinism is in the *rule check*; the *rule set's* coverage and tuning are judgment calls.

### What has changed recently (last 5 years)
- **Structured output went from prompt-hope to API-guarantee.** Provider-level structured-output / JSON-mode with constrained decoding replaced "please return JSON" prompting, turning a probabilistic ask into a deterministic shape contract.
- **Execution feedback moved into the training loop** (self-debug, RLEF), normalizing the idea that deterministic signals are the right grounding for code generation — not just an inference-time patch.
- **Secrets scanning and SAST became default CI gates** in mainstream platforms (GitHub/GitLab native), making the deterministic floor the expected baseline rather than an advanced practice.

---

## 3. Application Domain Examples
- **Code:** Compiler/typechecker (does it build? are types sound?) → linter (rule violations) → SAST (vulnerability patterns) → secrets scan (no committed credentials). All run in CI, all block merge, none require a human opinion. This is the cheapest, first layer of the code-validation stack completed in Ch. 3.
- **Structured data / tool calls:** JSON Schema / Pydantic / Zod validate the *shape* of an LLM's structured output before any downstream code touches it; constrained decoding prevents invalid shape at generation. Enforce structure deterministically first, then validate semantics separately (Ch. 6).
- **Config / IaC:** Schema validation of YAML/JSON config and policy-as-code (e.g., deterministic OPA-style rule checks) gate infrastructure changes generated by an agent before they apply.

---

## 4. The Book's Thesis Connection
This chapter is the thesis's first *positive* move: **start where ground truth is mechanically available.** The thesis says validation *works* where ground truth is mechanically present — and the deterministic floor is precisely the set of output properties where that is true *by construction*: well-formedness (compiler/typechecker), structural conformance (schema), rule-matched patterns (linter/SAST/secrets). Because the verdict is exact and repeatable, neither the model's self-assessment (which Ch. 1 showed is unreliable) nor the human's bias-prone attention (also Ch. 1) is the load-bearing check. The chapter also seeds the thesis's honest limit: the floor catches *form*, not *intent* — so it is *necessary and cheap but not sufficient*, which is exactly why the book then layers tests, retrieval grounding, judges, PRMs, and humans on top, output type by output type.

---

## 5. The AI Wayback Machine — Candidate Figures

- **Frances E. Allen (full Wikipedia name: Frances Allen).** American computer scientist, first woman to win the Turing Award (2006), pioneer of compiler optimization and program analysis at IBM. Her work is the deep ancestry of *static analysis* — reasoning about a program's properties from its source without running it, which is exactly what linters and SAST do. Diversifies the set (woman, compilers/static-analysis lineage). *Anchor prompt:* "Explain how static program analysis — Frances Allen's lineage — lets a tool prove properties of code without executing it, and why that makes a linter or SAST check deterministic."

- **Niklaus Wirth (full Wikipedia name: Niklaus Wirth).** Swiss computer scientist, Turing Award 1984; designer of Pascal and an evangelist of *strong static typing* as a discipline that catches whole classes of error at compile time. The typechecker as a judgment-free validator descends from this view. *Anchor prompt:* "Why did Wirth argue that a strict type system is a feature, not a constraint — and how is a typechecker a deterministic validator of program structure?"

- **Tim Bray (full Wikipedia name: Tim Bray).** Canadian software developer, co-editor of the XML specification and a foundational figure in machine-checkable data formats and (later) JSON's ecosystem. Embodies the idea of a *deterministic data contract*: a document is valid against the schema or it isn't. Lesser-known to ML readers; contemporary; diversifies discipline (standards/data, not algorithms). *Anchor prompt:* "Explain why a schema (XML Schema, JSON Schema) is a deterministic contract — a document validates or it doesn't — and how that differs from asking a model whether the data 'looks right.'"

*Diversity note:* better gender balance than Ch. 1 (Allen), but still skews mid-20th-century and Western; three computer scientists, though spanning distinct subdisciplines (compilers, language design, data standards). Era skew is again acceptable: the point is that the deterministic-validation *toolchain* long predates LLMs and is the stable ground the book stands on.

---

## 6. Pedagogical Delivery Research
- **Open with the contrast the reader already trusts:** they would never ship code that doesn't compile, but they'll ship an LLM answer that "reads right." Name the asymmetry: we already have a judgment-free gate for code (the compiler) — extend that instinct to *all* AI output by asking "what's the compiler-equivalent here?"
- **Teach the move, not the tool list:** for any output type, ask *"what property can be checked deterministically, and what tool returns that verdict?"* (shape → schema; build → compiler; pattern → SAST). The transferable skill is finding the floor.
- **Hands-on artifact (per TIKTOC, first artifact lands here):** have the reader wire a minimal CI gate — typecheck + lint + schema-validate an AI-produced output — and watch it block a fluent-but-broken example. The lesson sticks when the gate rejects something that "looked fine."
- **Pre-empt the over-claim:** immediately demonstrate a valid-shape-but-wrong-value document passing schema validation, so the reader internalizes "deterministic ≠ correct" before Ch. 3–6.

---

## 7. Representation and Display Research
**Layered-defense "deterministic floor" diagram.** A horizontal pipeline / stacked-gate figure showing AI output passing left-to-right through deterministic gates *before* any judgment-based layer:
1. **Generation** (AI output enters) →
2. **Parse / structural gate** — schema validation (JSON Schema / Pydantic / Zod); note constrained-decoding can move this *into* generation (a lock icon: "shape guaranteed") →
3. **Build gate** — compiler / typechecker (pass/fail) →
4. **Lint gate** — style + correctness rules →
5. **Security gate** — SAST + secrets scan (with a small "FP/FN tuning" caption) →
6. **dashed boundary line labeled "DETERMINISTIC FLOOR — judgment-free, blocks merge"** →
7. beyond the line, greyed-out: **tests → LLM-judge → human** (labeled "later chapters; judgment required").
Color/encoding: green check = exact pass/fail; the dashed boundary is the key visual — everything left of it is repeatable and unambiguous; everything right of it requires judgment. A caption notes each gate catches *form/pattern*, not *intent*.

---

## 8. Open Questions and Research Gaps
- **How far up the output-type ladder can deterministic checks reach** before judgment becomes unavoidable? (e.g., semantic schema constraints, property-based tests as a bridge.)
- **Optimal CI gating policy** under SAST false-positive load — what severity blocks vs. flags — lacks a principled, evidence-based standard; mostly folklore and vendor defaults.
- **Constrained-decoding cost/quality tradeoffs:** does forcing the FSM degrade content quality while guaranteeing shape? Evidence is mixed and model-dependent.
- **Coverage of secrets/SAST rule sets against AI-introduced vulnerability classes** (e.g., novel insecure patterns from code models) is an open, fast-moving area.

---

## 9. Sourcing Notes
- **Strongly verified (primary venues):** Huang et al. 2024 (ICLR; arXiv:2310.01798); Chen et al. "Teaching LLMs to Self-Debug" (ICLR 2024); RLEF (arXiv:2410.02089); Floyd 1967 / Hoare 1969 lineage (shared with Ch. 1); JSON Schema / Pydantic / Zod are stable, well-documented tooling; constrained-decoding-via-FSM is well-established in production structured-output APIs.
- **Flagged [verify]:** SAST scale/noise figures (~20k findings, ~1k FP per ~1M LOC; ~5% FP for best tools) are vendor/industry illustrative numbers, not peer-reviewed constants. Execution-feedback accuracy gains are real in direction but benchmark/model-specific — cite the mechanism, not a single delta.
- **Future-dated arXiv caution:** adjacent results surfaced with IDs implying late-2025/2026 (e.g., arXiv:2601.00509 secure-code RAG+multi-tool feedback; arXiv:2603.03305 draft-conditioned constrained decoding; arXiv:2603.10558 SAST false-positive prediction; arXiv:2604.03253 self-execution simulation; arXiv:2412.14841 feedback from testing+static analysis). Relative to 2026-05-29, IDs ≥ 2606 would be future-dated; the listed 2412–2604 IDs are plausibly real but were not individually fetched/verified — confirm before citing. arXiv:2412.14841 (Dec 2024) is the most directly usable and most plausibly verifiable of these.
- **No fetches were blocked.** Searches only (per instructions: no curl/python).
