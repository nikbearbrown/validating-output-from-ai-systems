# Research: Chapter 03 — Validating Generated Code
## Validating Output from AI Systems
**Chapter one-line:** Build a layered code-validation stack (compiler → tests → SAST → bounded diff review) and explain why iterative LLM "self-improvement" degrades security; tests validate the spec, not the intent.
**Research date:** 2026-05-29

---

## 1. Primary Sources

### Foundational papers and texts

- **DeMillo, R.A., Lipton, R.J., & Sayward, F.G. (1978). "Hints on Test Data Selection: Help for the Practicing Programmer." *Computer* 11(4), 34–41.** The founding paper of mutation testing. Establishes the idea this chapter leans on hardest: *coverage is not detection*. A test that executes a line proves nothing about whether the test would notice if that line were wrong. Mutation testing perturbs the program (flip `>` to `>=`, delete a statement) and asks whether the suite "kills" the mutant — the mutation score is the fraction killed. This is the precise diagnostic for the chapter's central code-validation failure: AI-generated tests that hit a coverage gate while asserting nothing meaningful. (Origin lore: the idea was Lipton's 1971 student term paper; first tool was Tim Budd's 1980 Yale PhD.) Verified primary citation.

- **Beck, K. (2003). *Test-Driven Development: By Example*. Addison-Wesley.** Supplies the red→green stopping condition and the discipline that produces high-mutation-score suites: write the assertion *before* the code, so the test is a specification rather than a post-hoc shape that traces the implementation. Relevant as the structural fix to the coverage-vs-mutation gap. (Deep prior-art overlap with the coding-agents book Ch. 4 — cross-reference, don't reproduce.)

- **Software verification & validation lineage / Hoare logic (assertion as proof obligation).** The conceptual ancestor of "the test is the oracle." Useful for framing the chapter's thesis sentence: a deterministic validator (compiler, type checker, assertion) is a proof obligation the code must discharge; an LLM's self-judgment is not. Cite for framing, not as a single source.

### Key empirical cases

- **Veracode (2025). *GenAI Code Security Report*.** *VERIFIED.* The headline empirical spine. Across **80 curated coding tasks run on 100+ LLMs** in four languages (Java, JavaScript, Python, C#), AI-generated code introduced security flaws in **45% of cases**. **Java was worst at ~72%** failure; the other three languages clustered at **38–45%**. Models failed to defend against **cross-site scripting (CWE-80) in ~86%** and **log injection (CWE-117) in ~88%** of relevant tasks. Crucially: **newer/larger models were not meaningfully more secure** — the report frames this as structural, not a scaling-away problem. (Author/publisher: Veracode; press release via BusinessWire, 2025-07-30; an October 2025 update adds GPT-5 data.) These exact figures (45% / 72% / 38–45% / 86% / 88%) are confirmed against Veracode's own materials.

- **Aljohani, A. et al. (2025). "Security Degradation in Iterative AI Code Generation — A Systematic Analysis of the Paradox." arXiv:2506.11022 (peer-reviewed & accepted, IEEE-ISTAS 2025).** *VERIFIED.* The "don't iterate for security" evidence. Controlled experiment: **400 code samples across 40 rounds of "improvement"** using four prompting strategies. Finding: a **37.6% increase in critical vulnerabilities after just five iterations** — asking the LLM to "improve" code paradoxically makes it less secure as iterations compound. The paper argues for mandatory human/deterministic validation *between* iterations. (Note: the seed also referenced a "21.1% crypto errors from security prompts" figure — **[verify]**: not confirmed verbatim in the abstract/summary surfaced; treat as unconfirmed and check the PDF body before printing.)

- **Author(s) (2025). "Security Vulnerabilities in AI-Generated Code: A Large-Scale Analysis of Public GitHub Repositories." arXiv:2510.26103 (submitted 2025-10-30; also Springer chapter).** *VERIFIED.* The real-world counterpart to Veracode's lab study. **7,703 files** explicitly attributed to four AI tools (ChatGPT 91.5%, Copilot 7.5%, CodeWhisperer 0.5%, Tabnine 0.5%), analyzed with **CodeQL**, yielding **4,241 CWE instances across 77 vulnerability types**. Key numbers: **87.9% of files had no identifiable CWE-mapped vuln**, but **Python ran hot at 16.18–18.50%** vs **JavaScript 8.66–8.99%** and **TypeScript 2.50–7.14%**. The Python-vs-TypeScript gap is the chapter's "language matters" data point. (Confirms the seed's "Python 16–18% vs TypeScript 2–7%.")

- **Mutation-testing-of-AI-tests practitioner evidence (2025–2026).** Multiple writeups document the signature gap: AI-generated suites hitting **~80–93% line coverage but ~34–58% mutation score** — i.e., tests that run the code but don't *detect* faults. The structural fix is TDD; a feedback loop (feed surviving mutants back to the model) raised one suite from 70%→78%. Tooling map: **PIT** (Java), **Stryker** (JS/TS), **mutmut** (Python). Cite as practitioner corroboration of the DeMillo/Lipton/Sayward principle, not as a controlled study.

---

## 2. The Core Concept — State of the Field

### What is settled
- **Deterministic validators are unambiguous and cheap; run them first.** Compiler, type checker, linter, and SAST give external, reproducible verdicts that don't depend on model judgment. This is the least controversial application of the book's thesis.
- **AI-generated code carries a substantial, measurable security-defect rate** (Veracode 45%; GitHub-corpus 12% file-level), and **it is not melting away with model scale** (Veracode's larger-model finding).
- **Coverage ≠ fault detection.** Mutation testing (DeMillo/Lipton/Sayward 1978) settled this 45+ years ago; the AI-test era merely makes it urgent again, because LLMs are very good at producing tests that *cover* without *checking*.
- **Tests validate the specification you encoded, not the intent you held.** A green suite proves the code does what the tests say — not what you meant. The "intent gap" is real and has no oracle but the human.

### What is disputed
- **How much human-in-the-loop iteration helps vs. hurts.** 2506.11022 shows iterating *for security* degrades it; but iterating against *deterministic* signals (failing test, compiler error) is the productive loop. The dispute is about which feedback signal — model self-judgment (harmful) vs. external deterministic verdict (helpful).
- **Whether feeding SAST/mutation results back to the model is "validation" or just more generation.** Practitioner reports are optimistic; there's no controlled evidence it converges safely. Treat as emerging.
- **Cross-tool/cross-corpus generalizability of the GitHub numbers** — attribution is self-reported ("written by ChatGPT") and CodeQL has known recall limits, so 12% is a floor, not a true rate.

### What has changed recently (last 5 years)
- The question moved from "can LLMs write code" (yes) to "is the code *safe*" (often not), with the first large multi-model security benchmarks (Veracode 2025) and large field corpora (2510.26103) arriving in 2025.
- The "iterate to improve" intuition was empirically inverted for security (2506.11022, 2025).
- Mutation testing, long academic, re-entered practitioner discourse specifically as the antidote to AI-test coverage-gaming (2025–2026).

---

## 3. Application Domain Examples
1. **The layered stack in CI:** compile/typecheck (rejects nonsense) → run human-owned tests (rejects spec violations) → SAST/CodeQL (rejects known CWE patterns) → bounded diff review (human reads only the changed hunks). Each layer catches what the prior cannot.
2. **The XSS/log-injection blind spot:** Veracode's 86%/88% miss rates mean the *tests passing* tells you nothing about injection safety — only SAST does. Concrete argument for why SAST is a non-optional layer, not a nice-to-have.
3. **The "improve this code" trap:** developer pastes working code, asks the model to "make it more robust/secure" five times; 2506.11022 predicts more critical vulns, not fewer. Guard: don't loop the model on security — run a SAST gate and stop.
4. **Coverage-gate gaming caught:** AI suite reports 90% coverage; mutmut/PIT reveals 50% mutation score. The 40-point gap is the tell that the tests assert little. Guard: gate on mutation score for critical paths, not coverage.
5. **Language-aware risk budgeting:** generated Python gets a heavier SAST/review budget than generated TypeScript (2510.26103's 16–18% vs 2.5–7%). Risk allocation by language is defensible from data.

---

## 4. The Book's Thesis Connection
This is the chapter where the thesis is most mechanically clean — and where its dark half also appears. **Validation works where ground truth is mechanically available:** code has compilers (does it parse and type-check?), tests (does it satisfy the encoded spec?), and SAST (does it match a known-bad pattern?). These are deterministic oracles; you run them first because they don't require judgment and they don't get fooled by fluency. **But the thesis's failure clause shows up twice.** First, *tests validate the spec, not the intent* — a green suite is silent on whether you specified the right thing, and a high-coverage/low-mutation suite is silent on whether it would even notice a regression. Second, *iterative LLM self-improvement degrades security* (2506.11022): when you replace the deterministic verdict with the model's own judgment of its work, fluency wins and safety erodes. So the engineering discipline the chapter teaches is exactly the book's: deterministic validators first, layered by what each catches and misses, and never substitute model self-assessment for an external oracle.

---

## 5. The AI Wayback Machine — Candidate Figures

- **Richard Lipton** (full: *Richard Jay Lipton*) — originator of mutation testing (1971 term paper; DeMillo–Lipton–Sayward 1978). The deepest, least-obvious anchor for "coverage is not detection." Living (Georgia Tech), so per house rules treat as secondary and flag living-figure skew. Anchor prompt: *"Explain Lipton's mutation-testing insight and use it to diagnose an AI-generated test suite with 90% coverage and 50% mutation score."*
- **Frederick Sayward** (full: *Frederick G. Sayward*) — the least-cited of the mutation-testing trio; useful precisely because he is obscure, giving a non-celebrity historical anchor. Yale; co-author of the 1978 paper. Anchor prompt: *"Reconstruct what the 1978 'Hints on Test Data Selection' paper actually proposed and why it predicts the AI-test coverage-gaming problem."*
- **Tony Hoare** (full: *Charles Antony Richard Hoare*) — Hoare logic (preconditions/postconditions; the assertion as a proof obligation) is the formal ancestor of "a deterministic check is an obligation the code must discharge." Historical stature, living. Anchor prompt: *"Frame a compiler error and a SAST finding as failed Hoare proof obligations, and contrast with an LLM saying 'this code looks secure.'"*
- *Skew note:* all three living and Anglo-American (the mutation-testing lineage is narrow). Prefer Lipton/Sayward as the substantive, lesser-invoked anchors; flag the living-figure caveat and the geographic/gender skew of this particular lineage.

---

## 6. Pedagogical Delivery Research
- **Teach the stack as a sieve, defect-type by defect-type:** for each layer (compiler / tests / SAST / review), state exactly one defect class it catches and one it cannot. The reader should leave able to say "tests would never have caught the XSS — only SAST would."
- **Use the coverage-vs-mutation demo as the hook:** show a suite at 90% coverage, run mutation testing live, watch the score collapse to ~50%. The gap is more memorable than the definition.
- **Pair the "iterate to improve" exercise with its inversion:** have the reader ask a model to "make this more secure" three times and run SAST after each; let them watch the vuln count rise (the 2506.11022 effect in miniature). Predict→break→measure.
- **Dislodge the misconception** "tests pass, so it's correct/secure": green means *spec-satisfied*, not *intended*, and says nothing about injection safety. Anchor with the 86%/88% miss rates.

---

## 7. Representation and Display Research

**(a) Four-layer code-validation defense diagram.** A left-to-right sieve, each layer a labeled gate that drops a defect class:
- Gate 1 **Compiler / type checker** — drops: syntax errors, type mismatches, undefined symbols. *Deterministic, instant.*
- Gate 2 **Tests (human-owned)** — drops: spec violations (encoded behavior wrong). *Caveat banner: validates the spec, not the intent; coverage ≠ detection.*
- Gate 3 **SAST / CodeQL** — drops: known CWE patterns (XSS, injection, crypto misuse). *Annotate: catches the 86–88% XSS/log-injection class that tests miss.*
- Gate 4 **Bounded diff review (human)** — drops: design/intent errors in the changed hunks only. *Annotate: scope-limited; fluency risk highest here.*
Each gate labeled "catches / misses"; an arrow shows defects surviving to the next gate. The point of the image: no single gate is sufficient; the human gate is last and narrowest.

**(b) Security-by-iteration line chart.** X-axis: refinement iterations (0→5). Y-axis: critical-vulnerability count (indexed to 100 at iteration 0). A rising line to ~137.6 at iteration 5 (the 2506.11022 finding), annotated "asking the model to 'improve' security made it worse." Optional flat reference line labeled "deterministic SAST gate (no model self-iteration)." This is the chapter's signature counter-intuitive image.

---

## 8. Open Questions and Research Gaps
- **Does feeding SAST/mutation results back to the model converge safely, or does it hit the 2506.11022 degradation?** Practitioner optimism, no controlled evidence. Open.
- **What is the *true* field vulnerability rate?** The 12% GitHub figure rests on self-reported attribution and CodeQL recall limits — likely a floor. Needs ground-truth-labeled corpora.
- **Will the Veracode "scale doesn't help" finding hold for the next model generation?** Model-specific snapshot; the principle (deterministic-first) is stable, the numbers are not.
- **Automatic detection of under-specified oracles** (tests that pass while leaving behavior unspecified — the intent gap). No reliable oracle for specification correctness but the human; "intent formalization" is an active but immature research direction. [verify — see sourcing notes on future-dated IDs]
- **The "21.1% crypto errors from security prompts" claim** attributed to 2506.11022 is unconfirmed against the surfaced summary. [verify in PDF body]

## 9. Sourcing Notes
- **Verified primary, exact figures confirmed:** Veracode 2025 GenAI Code Security Report (45% overall; Java ~72%; others 38–45%; CWE-80 ~86%, CWE-117 ~88%; larger≠safer) — Veracode materials + BusinessWire 2025-07-30. IEEE-ISTAS 2025 / **arXiv:2506.11022** (37.6% critical-vuln increase after 5 iterations; 400 samples / 40 rounds / 4 strategies; peer-reviewed, accepted). **arXiv:2510.26103** (7,703 files; 4,241 CWEs / 77 types; 87.9% clean; Python 16.18–18.50% vs JS 8.66–8.99% vs TS 2.50–7.14%; CodeQL). DeMillo, Lipton & Sayward 1978 (*Computer* 11(4):34–41) — mutation testing origin, verified. Beck 2003 (TDD).
- **Practitioner-only (corroborative, not controlled):** AI-test coverage-vs-mutation-score gap (~80–93% coverage / ~34–58% mutation score), PIT/Stryker/mutmut tooling map, mutant-feedback loop 70→78%. Cite as illustration, not evidence.
- **Unconfirmed / [verify]:** the "21.1% crypto errors from security prompts" sub-figure attributed to 2506.11022 — not seen verbatim in summaries; check the PDF before printing. 
- **Future-dated / preprint flags:** "Intent Formalization: A Grand Challenge…" surfaced as **arXiv:2603.17150** and a RiSE-MSR blog dated 2026-04 — these post-date a normal cutoff; cite the *intent-gap concept* conservatively and flag the IDs as future-dated/unverifiable preprints. Do not treat any quantitative claims from them as established.
- **Overlap management:** Beck/TDD and reward-hacking/test-gaming are covered deeply in the coding-agents book (pantry Ch. 4 & Ch. 8). This chapter owns the *validation-method* treatment (the layered stack, mutation diagnostics, the iteration-degrades-security finding); cross-reference rather than duplicate.
