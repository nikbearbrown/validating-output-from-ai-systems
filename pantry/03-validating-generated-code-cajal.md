# CAJAL Figure Intelligence — Chapter 3 — Validating Generated Code

Source: chapters/03-validating-generated-code.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
5 figure candidates. Mixed density. The chapter is figure-rich: a four-gate sieve mechanism, a coverage-vs-detection quantitative gap, a counter-intuitive security-degradation line result, a per-language vulnerability comparison, and the CI assembly process — each warrants its own figure under per-concept triage.

## Figure 1: The four-gate sieve — what each layer catches and misses
Priority: Critical
Trigger: MC — a four-stage ordered pipeline (compiler → tests → SAST → bounded review) where each gate catches a distinct defect class the prior let through
Figure type: Process flowchart
Concept statement: Generated code passes through four ordered gates — compiler, tests, SAST, bounded diff review — each catching a defect class the previous one structurally cannot, with no gate sufficient alone.
Reader prior knowledge: Reader has met compilers, tests, SAST, and code review separately; they have not seen them arranged as a cost-ordered sieve where each mesh catches exactly what the prior mesh let through.
Source anchor: Section: 3.2 The stack as a sieve — what each layer catches and what it cannot

### Block 1 — Illustrae Paste Block
Create a left-to-right process flowchart of four sequential gate nodes: compiler/typechecker, tests, SAST/CodeQL, bounded diff review. Connect them with single-headed arrows in series. Below or beside each gate, show one small "catches" marker and one small "misses" marker, so the reader sees each gate as a partial sieve. Show the meshes narrowing left to right (the bounded-review gate is the narrowest). Use Blue (#0072B2) for the three deterministic gates (compiler, tests, SAST) as the primary anchor since they are the mechanical layers; use Orange (#E69F00) for the final human bounded-review gate as the distinct, judgment-bearing secondary layer; use Bluish Green (#009E73) for the "catches" markers and light gray for the "misses" markers. Keep to four gate nodes plus paired catch/miss markers — split if it exceeds eight labeled components. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated.
[C - Content] four sequential gate nodes (compiler, tests, SAST, bounded review); a catches-marker and a misses-marker per gate. Relationship to preserve: series ordering plus per-gate partial coverage, narrowing left to right.
[O - Organization] horizontal left-to-right flow, single-headed arrows in series; catch/miss markers paired with each gate.
[P - Presentation] flat vector, Okabe-Ito mapping — deterministic gates #0072B2, human bounded-review gate #E69F00, catches markers #009E73, misses markers light gray; uniform 1pt strokes; no text labels in image.
[E - Exclusions] no mutation-testing detail (that is Figure 2); no security-degradation curve; no per-language bars; no specific CWE codes; no SQL-injection code sample; no the responsibility-stack from Chapter 1.
This block is the highest-leverage part: exactly four gates in series, narrowing meshes, one catch + one miss marker each.

### Block 3 — Negative Prompt
mutation diagrams, line charts, bar charts, CWE codes, SQL code, code snippets, Chapter-1 stack layers, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Coverage is not detection — the mutation-score gap
Priority: Critical
Trigger: PQ — explicit magnitudes: ~80–93% line coverage vs ~34–58% mutation score, a ~40-point gap
Figure type: Statistical/quantitative
Concept statement: AI-generated test suites report high line coverage but a far lower mutation score — they execute the code without detecting whether it is wrong.
Reader prior knowledge: Reader treats coverage as a quality signal; they have not seen the measured gap between execution (coverage) and detection (mutation score) that exposes coverage as misleading.
Source anchor: Section: 3.3 Coverage is not detection — the mutation-testing diagnostic

### Block 1 — Illustrae Paste Block
Create a grouped bar chart with a y-axis starting at zero and running to 100 (percent). Show two paired bars for an AI-generated suite: one tall bar for line coverage (in the ~80–93% range) and one much shorter bar for mutation score (in the ~34–58% range). Make the vertical gap between the two bars the visual point — a clear span between the tall coverage bar and the short mutation-score bar. Use Sky Blue (#56B4E9) for the coverage bar (the legible, optimized-for target) and Vermillion (#D55E00) for the mutation-score bar (the lower, true-detection reality). Optionally bracket the gap between the two bar tops with a neutral light-gray span marker. The y-axis must begin at zero. Keep to two bars plus one gap marker. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in numeric labels.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated, y-axis from zero.
[C - Content] one tall coverage bar, one short mutation-score bar, one gap-span marker between their tops; y-axis percent from 0 to 100. Relationship to preserve: a large vertical gap between execution and detection.
[O - Organization] two adjacent vertical bars on a shared zero-based y-axis; a bracket or span marking the difference between their tops.
[P - Presentation] flat vector, Okabe-Ito mapping — coverage bar #56B4E9, mutation-score bar #D55E00, gap marker light gray; uniform 1pt strokes; no text labels or numeric ticks in image.
[E - Exclusions] no mutant-generation diagram; no the four-gate sieve; no security-degradation curve; no tool logos (PIT/Stryker/mutmut); no code samples; no third bar.
This block is the highest-leverage part: y-axis from zero, exactly two bars with a marked gap, no printed numbers.

### Block 3 — Negative Prompt
mutant diagrams, tool logos, code snippets, third bars, line charts, percentage text, numeric tick labels, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion, truncated y-axis

---

## Figure 3: The self-improvement trap — iterating for security makes it worse
Priority: Critical
Trigger: PQ — a directional magnitude over rounds: +37.6% critical vulnerabilities after five refinement iterations
Figure type: Statistical/quantitative
Concept statement: Asking a model to iteratively "improve" its own code for security increases critical vulnerabilities — rising across refinement rounds rather than converging toward safe.
Reader prior knowledge: Reader expects iteration to converge toward better; they have not seen the counter-intuitive measured result that self-judgment in the loop drifts vulnerability counts *upward* over rounds.
Source anchor: Section: 3.4 The self-improvement trap — why iterating for security makes it worse

### Block 1 — Illustrae Paste Block
Create a single line chart with a y-axis starting at zero representing count of critical vulnerabilities and an x-axis representing refinement round number from 0 to 5. Plot one rising line that climbs across the rounds, ending notably higher at round five than it began at round zero — the wrong-direction result. Mark the round-five endpoint as the emphasis point. Use Vermillion (#D55E00) for the rising vulnerability line, since the trend is the negative/blocking result the section warns against; mark the round-five endpoint with a slightly heavier Vermillion point; use neutral light gray for the round-zero baseline reference. The y-axis must begin at zero. Keep to one rising line plus a baseline reference and one emphasis point. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in numeric labels.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated, y-axis from zero.
[C - Content] one rising line over five refinement rounds, a round-zero baseline reference, one emphasis point at round five; x-axis rounds 0–5, y-axis vulnerability count. Relationship to preserve: monotonic upward drift across self-improvement rounds.
[O - Organization] standard Cartesian plot, x-axis horizontal (rounds 0→5), y-axis vertical from zero; single ascending line.
[P - Presentation] flat vector, Okabe-Ito mapping — rising line and endpoint #D55E00, baseline reference light gray; uniform 1pt strokes; no text labels or numeric ticks in image.
[E - Exclusions] no deterministic-vs-self-judgment loop diagram (keep that conceptual split out of this quantitative figure); no the four-gate sieve; no per-language bars; no model/robot iconography; no SAST-feedback loop arrows; no second descending line.
This block is the highest-leverage part: y-axis from zero, exactly one rising line over five rounds, no printed numbers.

### Block 3 — Negative Prompt
loop diagrams, robot icons, model avatars, feedback arrows, second curves, descending lines, code snippets, percentage text, numeric tick labels, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion, truncated y-axis

---

## Figure 4: Vulnerability density varies by language — budget review accordingly
Priority: Important
Trigger: PQ — comparative magnitudes: Python ~16–18.5%, JavaScript ~8.7–9%, TypeScript ~2.5–7.1% file-level CWE rates
Figure type: Statistical/quantitative
Concept statement: AI-generated code carries different vulnerability densities by language — Python runs roughly two-to-seven times higher than TypeScript — which licenses budgeting SAST and review effort by language risk.
Reader prior knowledge: Reader assumes generated code risk is uniform; they have not seen the field-corpus data showing a several-fold per-language spread that justifies non-uniform review budgets.
Source anchor: Section: 3.5 The empirical picture, and how to budget review by language

### Block 1 — Illustrae Paste Block
Create a horizontal bar chart (dot plot acceptable) with a value axis starting at zero representing file-level vulnerability rate. Show three bars, one per language — Python highest, JavaScript in the middle, TypeScript lowest — sorted descending so the several-fold spread is immediately legible. Use Vermillion (#D55E00) for the Python bar (highest risk, the cautionary case); Orange (#E69F00) for the JavaScript bar (intermediate, secondary); Bluish Green (#009E73) for the TypeScript bar (lowest risk, the favorable case). The value axis must begin at zero. Keep to three bars. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in numeric labels.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated, value axis from zero.
[C - Content] three bars (Python, JavaScript, TypeScript) on a shared zero-based value axis, sorted descending. Relationship to preserve: the several-fold descending spread across three languages.
[O - Organization] horizontal bars, longest at top, shortest at bottom; shared zero-based value axis.
[P - Presentation] flat vector, Okabe-Ito mapping — Python bar #D55E00, JavaScript bar #E69F00, TypeScript bar #009E73; uniform 1pt strokes; no text labels or numeric ticks in image.
[E - Exclusions] no Veracode per-language failure bars mixed in (keep this to the field-corpus three-language rate only); no the four-gate sieve; no mutation-gap bars; no language logos/mascots; no CWE-type breakdown; no fourth bar (C#).
This block is the highest-leverage part: value axis from zero, exactly three descending bars, no language logos, no printed numbers.

### Block 3 — Negative Prompt
language mascots, language logos, fourth bars, CWE breakdowns, line charts, percentage text, numeric tick labels, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion, truncated value axis

---

## Figure 5: Iterate against the deterministic signal, not the model's judgment
Priority: Important
Trigger: VG — a structural distinction between two loop topologies (loop closes on a SAST/test verdict vs. loop closes on the model's self-assessment) that is the chapter's operative rule
Figure type: Comparison panels
Concept statement: An improvement loop is productive only when it closes on an external deterministic verdict; closing the loop on the model's own self-assessment drifts toward plausibility, not safety.
Reader prior knowledge: Reader knows iteration and SAST individually; they have not seen the two loop *topologies* contrasted — same model, same iteration, but a different reference signal closing the loop.
Source anchor: Section: 3.4 — "not all iteration is harmful — only iteration against the model's own judgment"

### Block 1 — Illustrae Paste Block
Create a two-panel comparison, each panel a small cycle diagram. Left panel (productive): a model node, an external deterministic-verdict node (a tool/gate), and an arrow loop where the model's output goes to the deterministic gate and the gate's hard verdict feeds back to the model — a closed loop anchored on an external reference. Right panel (harmful): a model node whose output loops back into the same model node as its own self-assessment, with no external gate in the loop. Use Bluish Green (#009E73) for the external deterministic-verdict gate and its loop (the productive, trustworthy reference); use Blue (#0072B2) for the model nodes in both panels as the neutral anchor; use Vermillion (#D55E00) for the self-referential loop in the right panel (the harmful, ungrounded case). Keep to five components total. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated.
[C - Content] left panel: model node, external deterministic-gate node, loop between them; right panel: model node, self-referential loop, no external gate. Relationship to preserve: external-grounded loop vs. self-grounded loop.
[O - Organization] two equal side-by-side panels, each a single closed loop; left loop passes through an external gate, right loop closes on the model itself.
[P - Presentation] flat vector, Okabe-Ito mapping — deterministic gate and its loop #009E73, model nodes #0072B2, self-referential loop #D55E00; uniform 1pt strokes; no text labels in image.
[E - Exclusions] no vulnerability-count curve (that is Figure 3); no the four-gate sieve; no specific tool names; no code samples; no round counters; no human reviewer.
This block is the highest-leverage part: two loops, one through an external gate and one closing on the model, clearly distinguished by color.

### Block 3 — Negative Prompt
line charts, vulnerability counters, round numbers, tool logos, code snippets, the four-gate sieve, human figures, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, hand-drawn styles, sketch lines, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — four-gate sieve: VIDEO CANDIDATE — sequential-process criterion — an animation passing a defect-laden artifact through the four gates and showing which defects each mesh removes (and the injection surviving to a missing gate) would dramatize the "no gate is sufficient alone" point; the static flowchart conveys it but the survival-through-gates story is naturally temporal.
FIGURE 2 — mutation-score gap: STATIC SUFFICIENT — a two-bar gap reads instantly.
FIGURE 3 — self-improvement trap: VIDEO CANDIDATE — progressive-build criterion — animating the vulnerability count climbing round by round would make the counter-intuitive upward drift land harder than a static line, though the zero-based static line already carries it.
FIGURE 4 — per-language density: STATIC SUFFICIENT — a three-bar comparison is a single-glance read.
FIGURE 5 — loop topologies: STATIC SUFFICIENT — the two closed loops are steady structures; animation would not add to the external-vs-self contrast.

Video candidates: 2. Recommended for production: Figure 1 (the four-gate sieve) — animating a defect surviving gate by gate until it hits the missing security gate is the chapter's narrative spine and benefits most from temporal staging; Figure 3 is a secondary option but the static line is sufficient.
