# CAJAL Figure Intelligence — Chapter 5 — Validating Reasoning Chains

Source: chapters/05-validating-reasoning-chains.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
4 figure candidates. Mechanistic density. The chapter is organized around a single branching decision (verify-by-domain) plus contrasting mechanisms (self-correction failure, self-consistency, the three-rung ladder) that are inherently structural and reward figural treatment.

## Figure 1: Verify-by-Domain Decision Tree
Priority: Critical
Trigger: MC + VG — a ≥3-branch interdependent decision (oracle? → comparable answer? → labelable steps?) terminating in distinct validators, with one branch explicitly forbidden; a structural decision not depictable from prose
Figure type: Process flowchart
Concept statement: The choice of reasoning validator is dictated by one repeated question — is ground truth mechanically available, and at what granularity — branching to proof checker, executor, self-consistency, PRM, or human, with model-self-critique-alone forbidden at every leaf.
Reader prior knowledge: Reader has met each validator individually (§5.2–5.5); has not seen them assembled into the single decision tree with the forbidden branch drawn and crossed out.
Source anchor: Section: 5.5 The decision: verify by domain

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector decision-tree flowchart, single column, white background, flowing top to bottom. Begin with a root decision diamond in blue #0072B2 (deterministic oracle?). Its yes branch splits into two solid-edged terminal boxes in bluish green #009E73 (proof/type checker, and executor). Its no branch leads down to a second decision diamond in blue #0072B2 (comparable final answer?), whose yes branch reaches a green #009E73 terminal (self-consistency). Its no branch leads to a third decision diamond (labelable steps?), whose yes branch reaches a green #009E73 terminal (process reward model) and whose no branch reaches an orange #E69F00 terminal (human review). Separately, off to one side, draw one disconnected forbidden terminal box in vermillion #D55E00 with a single diagonal strike-through (model self-critique alone), reachable from nowhere. Keep exactly three decision diamonds and five terminal boxes — eight components. Green = grounded leaves, orange = weakest legitimate leaf, vermillion struck = forbidden. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Three decision diamonds (oracle? / comparable answer? / labelable steps?) and five terminals (proof checker, executor, self-consistency, PRM, human) plus one disconnected struck-through forbidden terminal (self-critique). Preserve: the cascade order; every legitimate leaf is externally grounded (green) except human (orange, weakest); the forbidden leaf connects to nothing.
[O - Organization] Top-to-bottom tree; yes branches resolve to terminals, no branches descend to the next diamond; forbidden box isolated to the side with a strike-through.
[P - Presentation] Flat vector, Okabe-Ito: blue #0072B2 (decision diamonds), bluish green #009E73 (grounded terminals), orange #E69F00 (human terminal), vermillion #D55E00 (forbidden terminal, struck), black #000000 (connectors). 1pt strokes, no text in image.
[E - Exclusions] No validator names, no branch labels ("yes/no"), no question text, no human figures, no shadows, no gradients, no red-green adjacency on connected branches.

### Block 3 — Negative Prompt
validator names, "yes/no" branch labels, question text, "PRM/Lean/PAL" words, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Ungrounded Self-Correction Has No Gradient
Priority: Important
Trigger: VG + MC — a mechanism claim (a correction step needs an external gradient; self-critique loops back to its own plausibility) contrasted against grounded correction; structural, not depictable from text
Figure type: Comparison panels
Concept statement: A correction step needs a gradient pointing toward "more correct"; grounded correction gets it from an external signal (test, checker), while ungrounded self-critique loops back to the same plausibility estimate that produced the error.
Reader prior knowledge: Reader knows intrinsic self-correction can degrade accuracy (Huang et al.); has not seen the contrast drawn as one loop closing on itself versus one loop closing on an external oracle.
Source anchor: Section: 5.2 Why ungrounded self-correction fails

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector two-panel comparison, single column, white background, panels side by side or stacked. Left panel (ungrounded): a model box in orange #E69F00 with a single arrow leaving its output and curving directly back into its own input — a closed self-loop — colored vermillion #D55E00 to mark it as no-gradient. Right panel (grounded): the same model box in blue #0072B2 sends its output to an external oracle box (a checker/interpreter) in bluish green #009E73, which returns a feedback arrow into the model input — a loop that passes through the external node. Make the right loop's return arrow green #009E73 to mark a real gradient. Keep the two panels visually parallel so the only difference is whether the loop passes through an external node. Exactly: two model boxes, one external oracle box, two return loops — five components. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Left panel: model with a self-loop returning to its own input (no external node). Right panel: model whose loop passes through an external oracle node that supplies feedback. Preserve: the only structural difference is the presence/absence of the external node in the correction loop.
[O - Organization] Two parallel panels (left/right or top/bottom); each shows a model box and a return loop; the right loop routes through an added oracle box.
[P - Presentation] Flat vector, Okabe-Ito: orange #E69F00 (ungrounded model), blue #0072B2 (grounded model), bluish green #009E73 (external oracle + grounded return arrow), vermillion #D55E00 (ungrounded self-loop arrow), black #000000 (forward arrows). 1pt strokes, no text in image.
[E - Exclusions] No "self-correction/oracle/test" words, no model names, no numbers, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
component names, "model/oracle/feedback" words, accuracy numbers, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: Self-Consistency — Sample, Diverge, Vote
Priority: Important
Trigger: MC + VG — a multi-step aggregation mechanism (sample N paths → extract answers → majority vote) plus the structural claim that idiosyncratic errors scatter while correct chains converge
Figure type: Systems diagram
Concept statement: Self-consistency samples many independent reasoning paths, extracts each final answer, and takes the majority vote — correct chains converge on one destination while idiosyncratic errors scatter, so agreement correlates with correctness without any model self-assessment.
Reader prior knowledge: Reader knows chain-of-thought; has not seen the convergence-vs-scatter intuition (and its failure under shared bias) depicted as fanning paths collapsing onto a vote.
Source anchor: Section: 5.4 No oracle, comparable answer: self-consistency

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector systems diagram, single column, white background, flowing left to right. Start with one prompt node in light gray on the left. From it, fan out several (five or six) diverging path lines in blue #56B4E9, each ending in a small answer token. Draw most tokens converging onto one shared destination node on the right in bluish green #009E73 (the majority answer), with their lines bundling together. Draw one or two outlier tokens landing on separate small scattered nodes in light gray, off the main bundle, with their lines clearly diverging — the idiosyncratic errors. At the convergence point, render a simple tally/stack mark in black #000000 to imply counting. Keep exactly: one prompt, several path lines, one majority node, two scatter nodes, one tally mark — at most seven labeled components. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] One prompt; several diverging sampled paths; a majority of answer tokens converging on one destination node; one or two scattered outliers; a tally mark at the convergence. Preserve: independent paths fan out, most converge (correct), few scatter (idiosyncratic error), and the vote is a count at the convergence.
[O - Organization] Left-to-right; prompt fans into multiple paths; paths bundle onto one majority node on the right; outliers peel off to separate small nodes.
[P - Presentation] Flat vector, Okabe-Ito: light gray (prompt + outlier nodes), blue #56B4E9 (sampled path lines/tokens), bluish green #009E73 (majority node), black #000000 (tally mark). 1pt strokes, no text in image.
[E - Exclusions] No "vote/sample/N=20" text, no numerals, no code, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
vote counts, "N=20" or sample text, numerals, code snippets, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: The Validation Ladder — Three Rungs from None to Sound Oracle
Priority: Supplementary
Trigger: VG — a hierarchy/progression claim (weakest → strongest validation) that orders the chapter's mechanisms by strength; structural and not depictable from prose
Figure type: Timeline/progression
Concept statement: For one computational problem, validation escalates through three rungs — single greedy chain (no validation), self-consistency (statistical robustness), PAL execution (sound deterministic oracle) — and you climb as high as ground truth lets you.
Reader prior knowledge: Reader has each method; has not seen them ranked as an explicit strength ladder where some problems can reach the top rung and others cannot.
Source anchor: Section: 5.6 The same problem, three ways

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector progression/ladder, single column, white background, read bottom to top as increasing validation strength. Draw three stacked rungs as horizontal bars of increasing width or solidity. Rung one at the bottom (single greedy chain, no validation) in light gray with a thin, dashed, fragile edge. Rung two in the middle (self-consistency, statistical) in blue #56B4E9 with a solid edge. Rung three at the top (PAL execution, sound oracle) in bluish green #009E73 with a heavy solid edge. Place a single upward arrow in black #000000 along the left side spanning all three rungs to indicate increasing strength. Keep exactly three rungs and one ascent arrow — four components. Edge weight encodes validation strength (fragile dashed → heavy solid). 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Three ordered rungs (none / statistical / sound oracle) and one ascent arrow. Preserve: bottom-to-top order is weakest-to-strongest; edge weight rises with strength.
[O - Organization] Three horizontal bars stacked vertically, ascending; one vertical upward arrow on the left spanning the stack.
[P - Presentation] Flat vector, Okabe-Ito: light gray (no-validation rung, dashed fragile edge), blue #56B4E9 (statistical rung, solid edge), bluish green #009E73 (oracle rung, heavy edge), black #000000 (ascent arrow). 1pt strokes, no text in image.
[E - Exclusions] No rung names, no method labels, no numbers, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
rung names, method labels, "PAL/self-consistency" words, numbers, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — verify-by-domain decision tree: STATIC SUFFICIENT — a decision tree is a reference object the reader scans and re-scans; motion would hinder, not help.
FIGURE 2 — self-correction gradient: VIDEO CANDIDATE — the contrast is a loop that either closes on itself or routes through an oracle; a short animation tracing each loop (and the ungrounded one spinning without progress) would make the "no gradient" point visceral.
FIGURE 3 — self-consistency vote: VIDEO CANDIDATE — sampling-and-converging is inherently temporal; animating paths fanning out and tokens dropping onto the majority node (then showing systematic bias making them all converge wrong) is a natural explainer.
FIGURE 4 — validation ladder: STATIC SUFFICIENT — a strength ranking is a static reference.
Recommendation: Build all four as static; the decision tree (Figure 1) is the chapter's anchor and must be static and crisp, while Figures 2 and 3 are the strongest video upgrades if resources allow.
