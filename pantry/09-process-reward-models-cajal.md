# CAJAL Figure Intelligence — Chapter 9 — Process Reward Models: Step-Level Verification

Source: chapters/09-process-reward-models.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
5 figure candidates. Mechanistic density. The chapter is built on a single sharp mechanism (one-signal-at-end vs. one-signal-per-step) plus a Monte-Carlo labeling procedure, a verifier-vs-RL asymmetry (quantitative), and a domain boundary — each a distinct figure.

## Figure 1: ORM vs. PRM — One Signal at the End vs. One Per Step
Priority: Critical
Trigger: VG — the structural difference (endpoint scalar vs. per-prefix scores) is the chapter's core organizational claim
Figure type: Comparison panels
Concept statement: An ORM emits a single reward determined only by the final answer, spreading one bit of signal over all steps, whereas a PRM emits one score per step, letting it reject a right-answer/wrong-reasoning solution and localize the first bad step.
Reader prior knowledge: Reader has the §9.2 formal contrast and the §9.1 worked example; has not seen the two scoring schemes drawn over one shared step chain.
Source anchor: Section: 9.2 The mechanism: one signal at the end versus one signal per step

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector figure with two stacked panels sharing one left-to-right chain of five step nodes connected by single-headed arrows and ending in a terminal answer node. In the top panel ("ORM"), draw a single score readout (one short magnitude bar) attached only to the terminal answer node in Blue #0072B2, with the five preceding step nodes in neutral light gray to show they receive no own signal. In the bottom panel ("PRM"), draw a small score readout attached to every step node, each a short magnitude bar; render four of them Bluish Green #009E73 (valid steps) and one — the second step — Vermillion #D55E00 with a low bar (the caught wrong-reasoning step), while the terminal answer node still reads correct in Bluish Green #009E73. Align the five step nodes across both panels on a common baseline so endpoint-only vs. per-step is directly comparable. Limit to the shared chain plus the score readouts. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Shared 5-step chain to a terminal answer; ORM panel scores only the terminal; PRM panel scores every step, with step two flagged low/Vermillion while the answer is still correct. Preserve: endpoint-only vs. per-step, and right-answer/wrong-reasoning catch.
[O - Organization] Two stacked panels over an aligned left-to-right chain; score readouts as short magnitude bars on the relevant nodes.
[P - Presentation] Flat vector, Okabe-Ito: ORM terminal score Blue #0072B2, valid PRM steps Bluish Green #009E73, flagged bad step Vermillion #D55E00, unscored nodes neutral light gray; 1pt strokes; no text in image.
[E - Exclusions] No step numbers, no "ORM"/"PRM" text, no numeric scores, no equations, no legend.

### Block 3 — Negative Prompt
step numbers, ORM/PRM labels, numeric scores, equations, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Monte-Carlo Step Labeling by Rollout
Priority: Critical
Trigger: MC — the labeling procedure is ≥3 interdependent steps (pick step → sample k continuations → run to answer → aggregate fraction)
Figure type: Process flowchart
Concept statement: A step's value is estimated automatically by sampling k continuations from it, running each to a final answer, and taking the fraction that reach the correct answer — a survivability proxy that is cheap but noisy.
Reader prior knowledge: Reader has the §9.3 rollout formula; has not seen the branch-and-aggregate procedure drawn.
Source anchor: Section: 9.3 Getting step labels for free: Monte-Carlo rollouts

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector process diagram. Start with a single origin step node on the left in Blue #0072B2. From it, draw k (use five) single-headed arrows fanning out to the right into five short continuation paths, each path a small chain of two or three nodes ending in a terminal outcome marker. Render three of the terminal outcome markers in Bluish Green #009E73 (reached the correct answer) and two in Vermillion #D55E00 (did not). Then draw a single-headed arrow from the fan of outcomes into an aggregation node on the far right showing a magnitude bar filled to three-fifths in Orange #E69F00 (the empirical step value). The visual story: one step, five rollouts, a fraction-correct readout. Limit to: origin node, five continuation paths, five outcome markers, one aggregation readout. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] One origin step, five fan-out continuations, terminal outcome markers (3 correct / 2 not), an aggregation node showing a 3/5 magnitude bar. Preserve: rollout fan-out and fraction-correct aggregation.
[O - Organization] Left origin, fan-out continuations to the right, outcomes converging into a single aggregation readout.
[P - Presentation] Flat vector, Okabe-Ito: origin step Blue #0072B2, correct outcomes Bluish Green #009E73, incorrect outcomes Vermillion #D55E00, aggregated value bar Orange #E69F00; 1pt strokes; no text in image.
[E - Exclusions] No "k=5" text, no fraction numbers, no formula, no node labels, no legend.

### Block 3 — Negative Prompt
k labels, fraction numbers, formulas, node labels, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: PRM Use Asymmetry — Test-Time Verifier vs. RL Reward
Priority: Important
Trigger: PQ — the Math-Shepherd deltas (GSM8K 77.9→84.1, MATH 28.6→33.0) are magnitudes warranting a chart, y from zero
Figure type: Statistical/quantitative
Concept statement: Using a PRM as an RL reward signal yields only a few-point gain on GSM8K and MATH, whereas its use as a test-time best-of-N verifier yields a large lift — so the verifier role is where the leverage is.
Reader prior knowledge: Reader has the §9.3 deltas and the asymmetry rule; has not seen the small RL deltas charted against a zero baseline.
Source anchor: Section: 9.3 (Math-Shepherd RL deltas and the verifier-vs-RL asymmetry)

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector grouped vertical bar chart with a y-axis from zero to one hundred percent and light gray horizontal gridlines. Show two benchmark groups along the x-axis, each group a pair of adjacent bars (before vs. after RL). In the first group, a baseline bar near 78% and an after bar near 84%, both in primary Blue #0072B2 with the small increment portion topped in Orange #E69F00 to highlight the few-point RL gain. In the second group, a baseline bar near 29% and an after bar near 33%, same Blue base with an Orange increment cap. Keep all bars equal width, rising from a shared zero baseline, so the visually small Orange increments make the modest-RL point honestly. Limit components to four bars, the increment caps, axis, and gridlines. Use 1pt strokes, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Two benchmark groups (GSM8K ~78→84, MATH ~29→33), each a before/after pair, with the RL increment highlighted as a small cap. Preserve: zero-based axis and the visually small gains.
[O - Organization] Grouped vertical bars, two groups of two, y-axis 0–100%, even gridlines, increment caps on the "after" bars.
[P - Presentation] Flat vector, Okabe-Ito: bar bodies Blue #0072B2, RL-increment caps Orange #E69F00, gridlines/axis neutral light gray; 1pt strokes; no text in image.
[E - Exclusions] No value labels, no axis numbers, no benchmark names, no legend, no percent signs.

### Block 3 — Negative Prompt
value labels, axis numbers, benchmark names, percent signs, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: Where the PRM Stops — The Checkable-Step Boundary
Priority: Critical
Trigger: VG — the domain boundary (checkable step-truth vs. judgment) is the chapter's central structural claim
Figure type: Conceptual map
Concept statement: A PRM is a genuine step-level validator only in domains with checkable step-truth — math, formal proof, code — and collapses back into a bias-laden LLM-judge in open-ended domains where "correct step" is a matter of judgment.
Reader prior knowledge: Reader has the §9.5 domain run-down; has not seen the boundary drawn as a sorted map with a collapse on the far side.
Source anchor: Section: 9.5 Where it stops: the open-ended-task limit

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector conceptual map split by one vertical boundary line (neutral light gray) into a left region and a right region. On the left ("checkable step-truth"), place three domain tiles stacked vertically — competition math, formal proof, code — each filled Bluish Green #009E73 to mark genuine step-level validation, each with a small check-anchor glyph (a ground/tether mark) tying it to a solid baseline. On the right ("no checkable step"), place one domain tile (open-ended writing / strategy) filled Orange #E69F00 with its tether glyph cut/dangling (no ground), and beneath it draw a small transformation arrow (single-headed) leading to a judge node identical in shape to the left tiles but now drawn Blue #0072B2 to show the PRM has become an LLM-judge. The visual: grounded green on the left, ungrounded orange collapsing into a judge on the right. Limit to four domain tiles, one boundary, one transform arrow, one judge node. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Vertical boundary; left three grounded domain tiles (math, proof, code) with tether glyphs; right one ungrounded domain tile with a cut tether collapsing via an arrow into a judge node. Preserve: grounding present left, absent right, technique degenerates to a judge.
[O - Organization] Two regions split by a vertical line; tiles stacked; a single-headed transform arrow on the right region.
[P - Presentation] Flat vector, Okabe-Ito: grounded domains Bluish Green #009E73, ungrounded domain Orange #E69F00, collapsed judge node Blue #0072B2, boundary/tethers neutral light gray; 1pt strokes; no text in image.
[E - Exclusions] No domain names, no "checkable"/"judge" text, no captions, no legend, no dual-headed arrows.

### Block 3 — Negative Prompt
domain names, region labels, checkable/judge text, captions, legend, dual-headed arrows, text labels, words, gibberish letters, titles, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 5: PRM Localization in Search — Pruning the Bad Prefix
Priority: Supplementary
Trigger: MC — search-with-pruning is a multi-step branching process where per-step scores steer expansion
Figure type: Systems diagram
Concept statement: Because a PRM scores partial prefixes, a tree/beam search over partial solutions can prune a branch at the first low-scoring step instead of expanding it to a full wrong solution and discovering the failure only at the leaf.
Reader prior knowledge: Reader has the §9.2 localization-and-search point; has not seen prefix-pruning drawn as a pruned tree.
Source anchor: Section: 9.2 (Localization / search)

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector search tree branching top to bottom from a single root node, with each level adding a reasoning step. Draw two or three branches per node for two or three levels. Mark each node with a small per-step score chip: chips on the surviving path in Bluish Green #009E73, and at one early branch mark a low-score chip in Vermillion #D55E00; from that vermillion node draw a short pruning cut glyph (a single perpendicular slash on the outgoing edge) so the entire subtree below it is left undrawn/greyed in neutral light gray to show it was never expanded. The surviving green path continues to a leaf. The visual: an early bad prefix is cut, its subtree never grown, while a good prefix expands to completion. Limit to about seven nodes total plus score chips and one prune glyph. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Top-down search tree with per-step score chips; an early low-score node pruned (subtree greyed/undrawn); a surviving high-score path to a leaf. Preserve: pruning at the first bad prefix vs. expanding good prefixes.
[O - Organization] Branching tree, single-headed edges downward, score chips on nodes, one prune-cut glyph severing a subtree.
[P - Presentation] Flat vector, Okabe-Ito: good-prefix chips Bluish Green #009E73, pruned bad-prefix chip Vermillion #D55E00, unexpanded subtree neutral light gray, root/edges Blue #0072B2; 1pt strokes; no text in image.
[E - Exclusions] No step text, no score numbers, no node labels, no legend, no dual-headed arrows.

### Block 3 — Negative Prompt
step text, score numbers, node labels, legend, dual-headed arrows, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — ORM vs. PRM scoring: STATIC SUFFICIENT — two aligned panels over one chain are grasped at a glance; the flagged step is a fixed contrast.
FIGURE 2 — Monte-Carlo rollouts: VIDEO CANDIDATE — sampling continuations one by one and tallying the fraction is inherently sequential; animating the rollouts accumulating into the value bar shows the survivability proxy forming.
FIGURE 3 — verifier-vs-RL asymmetry bars: STATIC SUFFICIENT — grouped bars read instantly as a static chart.
FIGURE 4 — checkable-step boundary: STATIC SUFFICIENT — the sorted conceptual map and collapse are spatial, not temporal.
FIGURE 5 — search pruning: VIDEO CANDIDATE — search expanding and pruning is a temporal process; animating the prune-as-you-go dramatizes why a PRM steers search better than an ORM.
Recommendation: Build all five as static figures; promote Figures 2 and 5 to short animations where a video budget exists, since both encode genuinely sequential, accumulating processes (rollout tallying and search expansion/pruning).
