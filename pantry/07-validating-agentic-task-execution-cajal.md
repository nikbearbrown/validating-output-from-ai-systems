# CAJAL Figure Intelligence — Chapter 7 — Validating Agentic Task Execution

Source: chapters/07-validating-agentic-task-execution.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
5 figure candidates. Mechanistic density. The chapter turns on process structure (trajectory steps), a failure taxonomy, a timing-gated control flow, and two quantitative calibration claims — each independently warrants a figure.

## Figure 1: Endpoint vs. Trajectory Evaluation
Priority: Critical
Trigger: VG — endpoint-blindness is a structural claim ("the output is the path") not depictable from prose
Figure type: Comparison panels
Concept statement: Final-output evaluation inspects only the last state, whereas trajectory evaluation inspects the whole ordered sequence of (state, action, observation) steps, so a correct endpoint can hide a broken path.
Reader prior knowledge: Reader knows the (state, action, observation) step notation from §7.2; has not seen the two evaluation scopes contrasted side by side.
Source anchor: Section: 7.2 The trajectory is the unit: a taxonomy of path failures

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector comparison figure with two stacked horizontal panels sharing one left-to-right chain of five identical step nodes connected by single-headed arrows, each step node a small rounded rectangle representing a (state, action, observation) triple, ending in a larger terminal node for the final state. In the top panel ("endpoint evaluation"), draw a single evaluation bracket or magnifier glyph touching only the terminal node in Blue #0072B2, with the five preceding step nodes drawn in neutral light gray to signal they are unexamined. In the bottom panel ("trajectory evaluation"), draw one evaluation marker on every node including the terminal, all in Bluish Green #009E73, with a thin scope band spanning the whole chain. Keep all five step nodes aligned on a common baseline across both panels so the eye reads endpoint-only versus full-chain coverage. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Maximum eight components per panel. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Two panels over a shared 5-step (state/action/observation) chain plus a terminal final-state node; top panel marks only the terminal, bottom panel marks every node. Preserve: endpoint-only coverage vs. whole-chain coverage.
[O - Organization] Two stacked horizontal panels, left-to-right flow, single-headed arrows between nodes, nodes vertically aligned across panels on a common baseline, evaluation markers as small brackets/magnifier glyphs.
[P - Presentation] Flat vector, Okabe-Ito: examined-endpoint Blue #0072B2, full-trajectory Bluish Green #009E73, unexamined nodes neutral light gray; 1pt strokes; no text in image.
[E - Exclusions] No labels on nodes, no panel titles, no step numbers, no legend, no axis, no checkmarks-as-text.

### Block 3 — Negative Prompt
node text, step numbers, panel titles, legend entries, checkmark glyphs as letters, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: The Four Path-Failure Modes and Their Catchers
Priority: Critical
Trigger: VG — a taxonomy mapping four failure types to deterministic-vs-judge catchers is a hierarchy/organization claim
Figure type: Hierarchy/taxonomy
Concept statement: Four recurring trajectory failures — loop, scope violation, wrong-tool, error propagation — split across a deterministic/judge boundary, with two cleanly deterministic, one deterministic-if-specified, and two mixed.
Reader prior knowledge: Reader has read the §7.2 failure table; has not seen the deterministic/judge boundary drawn as a spatial partition.
Source anchor: Section: 7.2 The trajectory is the unit: a taxonomy of path failures

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector taxonomy with a single root node at top branching by single-headed arrows into four leaf nodes arranged left to right: loop, scope violation, wrong-tool, error propagation. Place a vertical partition cue (a thin dividing band running top to bottom) that sorts the leaves into a deterministically-catchable region on the left and a judge-required region on the right. Fill the two clearly deterministic leaves (loop; scope violation) in Bluish Green #009E73; fill the two mixed leaves (wrong-tool; error propagation) as split nodes, half Bluish Green #009E73 and half Orange #E69F00, to signal partial deterministic coverage. Draw the partition band in neutral light gray. Keep the root and four leaves to five primary components plus the partition. Use 1pt strokes, single-headed arrows only, all nodes as plain rounded rectangles of equal size, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] One root, four leaf nodes (loop, scope violation, wrong-tool, error propagation), a deterministic/judge partition band. Preserve: which leaves are fully deterministic vs. split/mixed.
[O - Organization] Root at top, four leaves in a row below via single-headed arrows; a vertical light-gray partition separating left (deterministic) from right (judge-needed).
[P - Presentation] Flat vector, Okabe-Ito: deterministic leaves Bluish Green #009E73, mixed leaves split Bluish Green #009E73 / Orange #E69F00, partition neutral light gray; equal-size rounded rectangles; 1pt strokes; no text in image.
[E - Exclusions] No leaf names, no "deterministic"/"judge" text, no table, no header row, no legend.

### Block 3 — Negative Prompt
leaf names, category text, deterministic/judge labels, table grid, header row, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: The Checkpoint Before Irreversibility
Priority: Critical
Trigger: MC — the gate is a ≥3-step interdependent control flow (classify action → branch → approve/deny → execute or reject)
Figure type: Process flowchart
Concept statement: Before executing each step the agent classifies the action as reversible or irreversible; irreversible actions route through a pre-execution approval gate whose denial blocks the side effect, while reversible actions flow straight through.
Reader prior knowledge: Reader has the §7.3 execute_step pseudocode and the "validate-before" idea; has not seen the branch-and-gate as a flow diagram.
Source anchor: Section: 7.3 The checkpoint before irreversibility: the one hard guarantee

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector process flowchart reading top to bottom. Start with an incoming-action node, then a diamond decision node testing reversibility, with two single-headed-arrow branches. The "reversible" branch routes directly down to a tool-execution node drawn in Bluish Green #009E73. The "irreversible" branch routes into a gate node (a distinct hexagon or shield shape) drawn in Orange #E69F00 to mark the checkpoint; from the gate, one single-headed arrow continues to the same execution node (approval passes through) and a second single-headed arrow turns to a blocked terminal drawn in Vermillion #D55E00 (denial stops the side effect). Keep the incoming node and execution node in primary Blue #0072B2. Limit to six nodes total: incoming, decision diamond, gate, execution, blocked terminal, and the reversible pass-through arrow. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Incoming action, reversibility decision diamond, pre-execution gate, tool-execution node, blocked terminal. Preserve: irreversible routes through gate before execution; denial terminates; reversible bypasses gate.
[O - Organization] Top-to-bottom flow; diamond with two labeled-by-position branches; gate with a pass-through arrow to execution and a turn-off arrow to a blocked terminal.
[P - Presentation] Flat vector, Okabe-Ito: incoming/execution primary Blue #0072B2, successful execution Bluish Green #009E73, gate Orange #E69F00, blocked terminal Vermillion #D55E00; gate as shield/hexagon, decision as diamond; 1pt strokes; no text in image.
[E - Exclusions] No node text, no "reversible/irreversible" words, no code, no yes/no labels on branches, no legend.

### Block 3 — Negative Prompt
node text, reversible/irreversible words, yes/no branch labels, code snippets, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: Error Propagation Through Steps
Priority: Important
Trigger: MC — a multi-step chain where one bad output silently feeds the next; ≥3 interdependent steps with downstream contamination
Figure type: Systems diagram
Concept statement: A wrong-but-well-formed value emitted at one step flows into the next step as if valid, so the error propagates downstream and contaminates every subsequent step rather than being isolated.
Reader prior knowledge: Reader knows the step chain and the §7.2 "error propagation" row; has not seen the silent-contamination flow visualized.
Source anchor: Section: 7.2 (error propagation row); 7.1 composite failure

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector systems diagram of a left-to-right chain of five step nodes connected by single-headed arrows, each node a rounded rectangle with a small output token feeding the next node's input. Draw the first two nodes in neutral primary Blue #0072B2 (clean steps). At the third node, mark a fault indicator (a small filled corner wedge) in Vermillion #D55E00 to show a wrong-but-well-formed value originates here. Then render the arrows and the remaining downstream nodes (steps four and five) tinted Vermillion #D55E00 to show the contamination propagating forward, with no error-raised break in the arrows (signal that the bad value is silent, not caught). Keep all five nodes on one baseline. Limit to five nodes plus the fault wedge. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Five-step chain, fault originating at step three, contamination flowing to steps four and five. Preserve: clean upstream, silent fault origin, contaminated downstream with unbroken arrows.
[O - Organization] Left-to-right single-baseline chain, single-headed arrows carrying output tokens between nodes, fault wedge at the origin node.
[P - Presentation] Flat vector, Okabe-Ito: clean steps Blue #0072B2, fault origin and contaminated downstream Vermillion #D55E00; 1pt strokes; no text in image.
[E - Exclusions] No step numbers, no value text, no error-symbol letters, no legend, no broken/dashed arrows implying a raised exception.

### Block 3 — Negative Prompt
step numbers, value text, error letters, exception symbols, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 5: Single-Run vs. 8-Run Reliability Collapse
Priority: Important
Trigger: PQ — the 60% single-run vs. 25% 8-run consistency figures are magnitudes warranting a bar chart, y-axis from zero
Figure type: Statistical/quantitative
Concept statement: The same agent that succeeds 60% of the time on a single run succeeds only 25% of the time under an 8-run consistency requirement, so single-run benchmark numbers overstate production reliability.
Reader prior knowledge: Reader has the §7.5 CLEAR finding and the compounding-over-horizon mechanism; has not seen the two numbers as a direct bar comparison.
Source anchor: Section: 7.5 The honest limit: benchmark success is not production reliability

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector vertical bar chart with exactly two bars on a shared baseline, y-axis starting at zero and running to one hundred percent. The left bar reaches 60% of full height and is filled in primary Blue #0072B2 (single-run success). The right bar reaches 25% of full height and is filled in Orange #E69F00 (eight-run consistency). Keep the two bars equal width with a clear gap between them, both rising from a common zero baseline so the relative heights read honestly. Draw light horizontal gridlines at even intervals in neutral light gray for reading the magnitudes. Do not annotate any value, axis tick, or bar with text. Limit components to the two bars, the baseline, the y-axis line, and the gridlines. Use 1pt strokes, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Two bars, 60% and 25% of a 0–100% axis, equal width, shared zero baseline, light gridlines. Preserve: zero-based axis and the ~2.4x height ratio.
[O - Organization] Two equal-width vertical bars with a gap, y-axis from zero to 100%, evenly spaced horizontal gridlines.
[P - Presentation] Flat vector, Okabe-Ito: single-run bar Blue #0072B2, eight-run bar Orange #E69F00, gridlines/axis neutral light gray; 1pt strokes; no text in image.
[E - Exclusions] No numeric value labels, no axis tick text, no bar captions, no title, no percent signs.

### Block 3 — Negative Prompt
value labels, axis numbers, tick text, percent signs, bar captions, title, legend, text labels, words, gibberish letters, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — endpoint vs. trajectory evaluation: STATIC SUFFICIENT — single structural contrast — two side-by-side coverage scopes are grasped at a glance with no temporal unfolding.
FIGURE 2 — failure taxonomy: STATIC SUFFICIENT — fixed hierarchy — a partition of four leaves does not benefit from motion.
FIGURE 3 — checkpoint flowchart: VIDEO CANDIDATE — sequential branching decision with a timing-critical "before vs. after" beat — an animation that walks an action to the gate and shows denial halting the side effect dramatizes the "last point a no still matters" idea.
FIGURE 4 — error propagation: VIDEO CANDIDATE — propagation over time is the whole concept — animating the fault spreading step by step shows silent contamination better than a static tint.
FIGURE 5 — reliability collapse: STATIC SUFFICIENT — two-bar magnitude comparison — reads instantly as a static chart.
Recommendation: Build all five as static figures; promote Figures 3 and 4 to short animated explainers if a video budget exists, as both encode genuinely temporal mechanisms.
