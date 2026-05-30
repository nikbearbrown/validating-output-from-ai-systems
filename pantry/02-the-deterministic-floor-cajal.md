# CAJAL Figure Intelligence — Chapter 2 — The Deterministic Floor: Validators That Don't Need Judgment

Source: chapters/02-the-deterministic-floor.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
4 figure candidates. Mechanistic density. The chapter is built on mechanisms and structural moves — the "find the floor" decision move, the constrained-decoding FSM gating mechanism, the floor-vs-ceiling boundary, and the SAST false-positive/false-negative tradeoff — most of which are not depictable from text alone.

## Figure 1: The "what's the compiler-equivalent here?" move
Priority: Critical
Trigger: VG — a repeatable mapping move (output property → tool) that is structural and reusable, not visible as a single sentence
Figure type: Conceptual map
Concept statement: For any AI output type, three property classes map to three tool classes — shape to schema validator, build to compiler/typechecker, pattern to SAST/linter.
Reader prior knowledge: Reader knows compilers and schema validators individually; they have not seen them unified as instances of one transferable move from "decidable property" to "tool that returns the verdict."
Source anchor: Section: 2.3 The move — find the floor for any output type

### Block 1 — Illustrae Paste Block
Create a conceptual map with a single central node on the left (the move: "what property can be checked deterministically?") fanning out via three single arrows to three property nodes — shape, build, pattern — each of which connects by a single arrow to its corresponding tool node — schema validator, compiler/typechecker, static analysis. Arrange as three clean parallel rows reading left to right: property → tool. Use Blue (#0072B2) for the central move node as the primary anchor; use Sky Blue (#56B4E9) for the three property nodes; use Bluish Green (#009E73) for the three tool nodes, since the tools are the active, trustworthy verdict-givers. Keep to seven components total (one central, three properties, three tools — collapse property/tool into paired cells if seven crowds). Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated.
[C - Content] one central move node, three property nodes (shape, build, pattern), three tool nodes (schema, compiler, SAST). Relationship to preserve: one move fans to three property→tool pairings.
[O - Organization] central node on the left; three horizontal rows to its right, each property node followed by a single arrow to its tool node; no cross-row links.
[P - Presentation] flat vector, Okabe-Ito mapping — central move #0072B2, property nodes #56B4E9, tool nodes #009E73; uniform 1pt strokes; no text labels in image.
[E - Exclusions] no constrained-decoding FSM (that is Figure 2); no CI pipeline imagery; no floor/ceiling layering; no false-positive tradeoff; no specific tool logos or file-format icons; no factual-claim citation row.
This block is the highest-leverage part: exactly three property→tool rows, no logos, no FSM.

### Block 3 — Negative Prompt
tool logos, file-format icons, FSM diagrams, CI pipeline stages, floor-ceiling layers, gauges, language mascots, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Constrained decoding makes invalid shape unrepresentable
Priority: Critical
Trigger: MC — a multi-step token-gating mechanism (schema → FSM → per-token masking → only-valid tokens emitted) with ≥3 interdependent components
Figure type: Mechanism cross-section
Concept statement: Compiling a schema into a finite-state machine lets the decoder mask, at each token step, any token that could not complete into a valid document — so structurally invalid output cannot be generated.
Reader prior knowledge: Reader knows "validate after generation"; they have not seen the check moved *into* generation as a per-step token mask, which is what makes invalid shape unrepresentable rather than merely rejected.
Source anchor: Section: 2.4 The purest case — constrained decoding makes invalid shape unrepresentable

### Block 1 — Illustrae Paste Block
Create a mechanism cross-section showing, from left to right: a schema source compiling (single arrow) into a finite-state-machine node; the FSM then governs a token-decoding step shown as a row of candidate token slots where some slots are open and others are masked out; a single arrow leads from the open (allowed) slots to a growing valid-output strip. Show the masking as the central mechanism: allowed tokens pass, disallowed tokens are blocked at the gate. Use Sky Blue (#56B4E9) for the schema and FSM nodes as the primary anchor; Bluish Green (#009E73) for the allowed/passing token slots and the valid-output strip (active/positive); Vermillion (#D55E00) for the masked/blocked token slots (blocking/negative); neutral light gray for the decoding-step frame. Keep to six components. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated.
[C - Content] schema node, FSM node, a row of token slots split into allowed and masked, a valid-output strip. Relationship to preserve: schema→FSM→per-token mask→only-valid output.
[O - Organization] left-to-right flow; schema → FSM at top-left, token-slot row in the center with allowed and blocked slots distinguished, valid-output strip on the right.
[P - Presentation] flat vector, Okabe-Ito mapping — schema/FSM #56B4E9, allowed slots and output strip #009E73, masked slots #D55E00, decoding frame light gray; uniform 1pt strokes; no text labels in image.
[E - Exclusions] no JSON text or braces rendered as readable characters; no compiler/typechecker (that is the floor table); no quality-cost tradeoff; no human reviewer; no the "what's-the-compiler" move map; no probability distributions.
This block is the highest-leverage part: the masking gate is the point — show allowed-vs-blocked token slots, no readable JSON.

### Block 3 — Negative Prompt
readable JSON, curly braces as text, compiler icons, probability bars, distribution curves, quality gauges, human figures, the move-map, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: The floor and its ceiling — form is checked, intent is not
Priority: Critical
Trigger: VG — a structural boundary claim ("deterministic checks cover form/pattern below the line, intent/semantics sit above it") not depictable from prose
Figure type: Comparison panels
Concept statement: Deterministic validators decide form and rule-matched patterns (below the line) but say nothing about intent or semantic truth (above the line) — the same artifact can be green on every check and still wrong.
Reader prior knowledge: Reader now trusts deterministic checks; they have not internalized the *boundary* — that a single horizontal ceiling separates everything the floor can decide from everything it structurally cannot.
Source anchor: Section: 2.5 The ceiling — form is not intent

### Block 1 — Illustrae Paste Block
Create a two-zone vertical panel divided by one bold horizontal boundary line across the middle (the ceiling). The lower zone (the floor) contains a small set of property markers representing decidable, mechanically-checked properties — well-formedness, structural conformance, rule-matched patterns — shown as solid, settled markers. The upper zone (above the ceiling) contains property markers representing intent and semantics — right values, right specification, sound argument — shown as open, unsettled markers. The horizontal boundary line is the load-bearing element. Use Bluish Green (#009E73) for the lower-zone decidable property markers (active, settled, trustworthy); use Orange (#E69F00) for the upper-zone intent/semantic markers (secondary, unresolved); use Black (#000000) for the bold horizontal ceiling line. Keep to six markers plus the dividing line. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated.
[C - Content] one bold horizontal boundary line; three solid property markers below; three open property markers above. Relationship to preserve: a single ceiling separating decidable-form from undecidable-intent.
[O - Organization] vertical split, one horizontal divider in the middle; settled markers below, unsettled markers above; no arrows crossing the line.
[P - Presentation] flat vector, Okabe-Ito mapping — below-line markers #009E73, above-line markers #E69F00, dividing ceiling line #000000; uniform 1pt strokes; no text labels in image.
[E - Exclusions] no CI pipeline; no FSM; no the move-map; no specific code or JSON; no false-positive numbers; no green-checkmark / red-X iconography (which risks red-green); no human reviewer figure.
This block is the highest-leverage part: exactly one horizontal ceiling line, settled markers below and unsettled above, nothing crossing it.

### Block 3 — Negative Prompt
checkmarks, X marks, traffic lights, CI pipeline stages, FSM diagrams, JSON text, code snippets, percentage figures, human figures, crossing arrows, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: SAST gating — the false-positive / false-negative tradeoff
Priority: Important
Trigger: PQ — magnitudes and a tunable tradeoff (~5% FP, ~20k findings / ~1k FP per ~1M LOC; tighten vs. loosen the rules)
Figure type: Statistical/quantitative
Concept statement: Tightening SAST rules catches more real vulnerabilities but raises false positives; loosening them cuts false positives but raises silent false negatives — the gate sits on a tunable tradeoff curve.
Reader prior knowledge: Reader knows false positives and false negatives as concepts; they have not seen them as opposing quantities that move in opposite directions as one tunes rule strictness.
Source anchor: Section: 2.5 The ceiling — SAST coverage tradeoff (and Exercise 3)

### Block 1 — Illustrae Paste Block
Create a single chart with a horizontal axis representing rule strictness (loose on the left, strict on the right) and a vertical axis starting at zero representing count of findings. Plot two opposing curves: one rising left to right (false positives, increasing as rules tighten) and one falling left to right (false negatives / missed real defects, decreasing as rules tighten). Mark the crossing region where the two curves intersect as the tradeoff zone. Use Orange (#E69F00) for the false-positive curve as the secondary quantity; use Vermillion (#D55E00) for the false-negative / missed-defect curve as the blocking/negative quantity; use a neutral light-gray vertical band to mark the intersection tradeoff zone. The vertical axis must begin at zero. Keep to two curves plus the intersection band. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in numeric labels.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated, y-axis from zero.
[C - Content] one rising false-positive curve, one falling false-negative curve, one intersection band; x-axis rule strictness, y-axis count. Relationship to preserve: two quantities moving in opposite directions across strictness.
[O - Organization] standard Cartesian plot, x-axis horizontal (loose→strict), y-axis vertical from zero; the two curves cross near the center.
[P - Presentation] flat vector, Okabe-Ito mapping — false-positive curve #E69F00, false-negative curve #D55E00, intersection band light gray; uniform 1pt strokes; no text labels or numeric ticks in image.
[E - Exclusions] no specific vendor numbers printed; no codebase or LOC iconography; no CI pipeline; no the floor/ceiling figure; no severity-level table; no third curve.
This block is the highest-leverage part: y-axis from zero, exactly two opposing curves and one intersection band, no printed numbers.

### Block 3 — Negative Prompt
printed percentages, LOC counters, severity tables, CI stages, code icons, third curves, bar charts, text labels, words, gibberish letters, titles, captions, numeric tick labels, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion, truncated y-axis

---

## Video Candidate Pass
FIGURE 1 — the move-map: STATIC SUFFICIENT — a property→tool mapping is a fixed lookup; nothing unfolds over time.
FIGURE 2 — constrained decoding FSM: VIDEO CANDIDATE — sequential-process criterion — token-by-token masking is inherently temporal; an animation that walks the decoder forward one token at a time, greying out blocked tokens as the FSM advances, would show "invalid becomes unrepresentable" as it happens; the static cross-section conveys the mechanism but the stepwise gating is the most animation-friendly concept in the chapter.
FIGURE 3 — floor and ceiling: STATIC SUFFICIENT — a structural boundary is a single steady relationship.
FIGURE 4 — FP/FN tradeoff: STATIC SUFFICIENT — the opposing curves read in one glance; a slider animation would add little over the static crossing.

Video candidates: 1. Recommended for production: Figure 2 (constrained-decoding token masking) — its stepwise, sequential nature makes animation genuinely clarifying in a way no other figure in this chapter matches; all others are well-served by static.
