# CAJAL Figure Intelligence — Chapter 11 — Building a Validation Pipeline

Source: chapters/11-building-a-validation-pipeline.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
4 figure candidates. Mechanistic density. The chapter is an assembly chapter built around one architecture (the layered Swiss-cheese pipeline), one matrix (failure-mode → catching layer), one cross-cutting system (the observability bus), and a worked instantiation — each a strong, distinct figure with the pipeline architecture as the Critical anchor.

## Figure 1: The layered validation pipeline (Swiss-cheese stack)
Priority: Critical
Trigger: MC — ordered ≥4-stage process with an enforced ordering rule (deterministic → automated → judge/PRM → human), interdependent because each slice handles only the residual the previous could not
Figure type: Systems diagram
Concept statement: A validation pipeline is a defense-in-depth stack of four slices ordered by ground-truth availability — deterministic first, human last — where each slice is a barrier with holes and an output reaches production only when the holes align.
Reader prior knowledge: They have met each validator individually across Ch. 2–10; they have not seen them ordered into one stack with the ground-truth-first ordering rule and the aligned-holes trajectory made visible.
Source anchor: Section: 11.2 The Swiss-cheese frame and the ordering rule

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector defense-in-depth diagram on a white background for a single-column textbook, showing four vertical slice-barriers arranged left to right in fixed order, each slice a tall rounded rectangle with two or three circular holes punched through it. A single straight horizontal arrow enters from the left and passes through one aligned hole in every slice, exiting on the right to mark the failure trajectory. Color the slices by ground-truth reliability: slice one (deterministic) in bluish green #009E73 to mark the only fully trustworthy oracle; slice two (automated) in sky blue #56B4E9; slice three (judge/PRM) in orange #E69F00; slice four (human) in vermillion #D55E00 to mark the thinnest, most degradable barrier. Draw the human slice visibly thinner than the others. Render the trajectory arrow in black #000000. Use exactly four slices plus one arrow, 1pt strokes, flat fills, evenly spaced slices, holes as simple open circles. Leave everything blank — no text, words, numbers, titles, or captions. No shadows, gradients, or decorative framing.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Four ordered slice-barriers (deterministic, automated, judge/PRM, human) each with holes, plus one horizontal trajectory arrow passing through one aligned hole per slice. Preserve left-to-right ordering and the thinner human slice.
[O - Organization] Four parallel vertical barriers in a row; single black arrow entering left, threading one hole in each, exiting right.
[P - Presentation] Flat vector, Okabe-Ito: #009E73 (deterministic), #56B4E9 (automated), #E69F00 (judge/PRM), #D55E00 (human, drawn thinnest), #000000 (trajectory arrow); 1pt strokes; holes as open circles; no text in image.
[E - Exclusions] No text, no cheese texture or photographic cheese, no more than four slices, no branching arrows, no color outside listed hexes, no hole alignment other than the single threaded trajectory.

### Block 3 — Negative Prompt
photographic cheese, cheese texture, slice count other than four, branching arrows, multiple trajectory lines, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Failure-mode → catching-layer matrix
Priority: Critical
Trigger: VG — a structural mapping ("which slice catches which mode, and which have no deterministic catcher") that is hierarchical/relational and central to the chapter
Figure type: Annotated example (matrix grid)
Concept statement: Eight named failure modes map onto four catching slices, and exactly four of the eight have no deterministic catcher — their only line of defense is a model or human slice, marking them as residual risk by construction.
Reader prior knowledge: They have seen the eight modes individually; they have not seen them laid against the four slices in one grid that exposes the four residual-by-construction modes as a visual cluster.
Source anchor: Section: 11.4 The failure-mode → catching-layer matrix

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector matrix grid on a white background for a single-column textbook, showing an 8-row by 4-column lattice of equal cells — eight failure-mode rows against four catching-slice columns (deterministic, automated, judge/PRM, human). Leave most cells empty; place one filled marker dot in the primary catching column of each row. Color each marker by the slice's reliability: deterministic-column markers in bluish green #009E73, automated-column markers in sky blue #56B4E9, judge/PRM-column markers in orange #E69F00, human-column markers in vermillion #D55E00. For the four rows whose only catcher is a model or human slice (the residual-by-construction modes), add a small open ring outline around that row's marker in vermillion #D55E00 to flag residual risk. Draw thin 1pt gridlines, equal cell sizes, flat fills, aligned columns and rows. Leave all cells, rows, and columns blank — no text, words, numbers, headers, titles, or captions. No shadows, gradients, or decoration.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] 8×4 grid; one marker per row in its primary catching column; four of the eight rows additionally ringed to flag no-deterministic-catcher residual risk. Preserve the count (8 rows, 4 columns, 4 ringed rows).
[O - Organization] Rectangular lattice, equal cells, markers as filled dots, residual flags as open rings around the relevant dots.
[P - Presentation] Flat vector, Okabe-Ito: #009E73 (deterministic column), #56B4E9 (automated column), #E69F00 (judge/PRM column), #D55E00 (human column markers and residual rings); 1pt gridlines; no text in image.
[E - Exclusions] No text, no row/column headers, no checkmark glyphs, no more than four ringed rows, no color outside listed hexes, no heatmap shading of empty cells.

### Block 3 — Negative Prompt
row headers, column headers, checkmark glyphs, X glyphs, heatmap shading, more than four ringed rows, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: The observability bus
Priority: Important
Trigger: VG — a structural/systems claim (a cross-cutting bus running underneath every slice, doing attribution and drift detection) not depictable from prose alone
Figure type: Systems diagram
Concept statement: An observability bus runs underneath every pipeline slice, capturing each slice's decision so a production failure is attributable to a specific slice and pass-rate drift is detectable over time.
Reader prior knowledge: They understand the four-slice pipeline; they have not seen the horizontal observability layer drawn as a bus tapping every slice from below.
Source anchor: Section: 11.3 The observability bus: making failure attributable

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector systems diagram on a white background for a single-column textbook, showing four slice nodes in a horizontal row across the top (the pipeline slices) and one long horizontal bus bar spanning the full width beneath them. Draw a single short vertical connector line from each of the four slice nodes down to the bus bar, marking the per-slice decision tap. Render the four slice nodes in neutral light gray to keep focus on the bus. Render the bus bar in blue #0072B2 as the primary cross-cutting system. At the right end of the bus, attach one small output element representing attribution/drift output in bluish green #009E73. Use at most six components, 1pt strokes, flat fills, equal spacing of the four taps, a single straight bus bar. Leave every node, the bus, and the connectors blank — no text, words, numbers, titles, or captions. No shadows, gradients, or decorative framing.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Four pipeline slice nodes in a row, one full-width bus bar beneath, four vertical taps connecting each slice to the bus, one attribution/drift output element. Preserve the underneath-everything spanning relationship.
[O - Organization] Top row of four nodes; single horizontal bus bar below; four equal vertical connectors; one output node at the bus's right end.
[P - Presentation] Flat vector, Okabe-Ito: light gray (slice nodes), #0072B2 (bus bar), #009E73 (attribution/drift output); 1pt strokes; no text in image.
[E - Exclusions] No text, no database cylinders or log icons, no more than four taps, no color outside listed hexes, no arrows other than the simple vertical taps.

### Block 3 — Negative Prompt
database cylinder icons, log file icons, gear icons, more than four taps, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: Worked pipeline — the §11.1 aligned-holes failure
Priority: Supplementary
Trigger: MC — a multi-step trajectory through four slices where each slice's specific hole passes the wrong output (schema → tests → judge → auto-approved human)
Figure type: Process flowchart
Concept statement: In the composite failure, a fluent wrong answer passes a shape-only schema check, a plumbing-only test suite, a fluency-correlated judge score above threshold, and an auto-approve gate that routed around the human — four holes in a row.
Reader prior knowledge: They have read the narrative; they have not seen the single output's path traced as a linear pass-through where each slice's hole is the reason it failed to catch.
Source anchor: Section: 11.1 The pipeline that had every layer and shipped the wrong answer

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector process flowchart on a white background for a single-column textbook, showing a left-to-right linear chain of four gate nodes connected by single-headed arrows, with one input node at the far left (the fluent wrong answer) and one output node at the far right (reaches the customer). Each of the four gate nodes (schema check, test suite, judge score, auto-approve gate) is drawn as a rounded rectangle with a single open notch or gap on one side to depict the hole the output passed through. Render the input node in sky blue #56B4E9, the four gate nodes in orange #E69F00 to mark passed-but-uncaught, and the terminal output node in vermillion #D55E00 to mark the harmful outcome. Keep all arrows single-headed and straight, the chain strictly linear with no branches. Use exactly six nodes, 1pt strokes, flat fills, even spacing. Leave every node blank — no text, words, numbers, titles, or captions. No shadows, gradients, or decoration.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Input node, four sequential gate nodes each drawn with a hole/notch, terminal harmful-output node; single linear pass-through trajectory. Preserve the four-in-a-row hole sequence and the strictly linear flow.
[O - Organization] Left-to-right linear chain; single-headed straight arrows; no branches.
[P - Presentation] Flat vector, Okabe-Ito: #56B4E9 (input), #E69F00 (four uncaught gates), #D55E00 (harmful terminal output); 1pt strokes; gates drawn with a single open notch each; no text in image.
[E - Exclusions] No text, no checkmark/X glyphs, no branching, no more than six nodes, no color outside listed hexes.

### Block 3 — Negative Prompt
checkmark glyphs, X glyphs, branching paths, more than six nodes, customer or person icons, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — layered pipeline / Swiss-cheese stack: VIDEO CANDIDATE — an output threading aligned holes through four ordered barriers is inherently sequential — a short animation of the trajectory passing slice by slice would dramatize the aligned-holes accident, though the static figure carries the full concept.
FIGURE 2 — failure-mode matrix: STATIC SUFFICIENT — a fixed relational lattice — the residual cluster is a spatial pattern best read at rest.
FIGURE 3 — observability bus: STATIC SUFFICIENT — a fixed structural relationship (bus under slices) — no temporal dimension to animate.
FIGURE 4 — §11.1 aligned-holes failure: VIDEO CANDIDATE — a single output's pass-through trajectory is sequential — step-by-step reveal would reinforce "four holes in a row," but the static linear chain conveys it.

Recommendation: Build all four as static figures; Figure 1 is the Critical architecture anchor and Figure 2 the Critical deliverable. If animation budget exists, spend it on Figure 1's trajectory threading the aligned holes — the single most concept-carrying motion in the chapter.
