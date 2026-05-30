# CAJAL Figure Intelligence — Chapter 8 — LLM-as-Judge: Uses and Failure Modes

Source: chapters/08-llm-as-judge-uses-and-failure-modes.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
5 figure candidates. Mixed density. The chapter blends one mechanism (position-bias-modulated-by-quality-gap), a structural circularity claim, a four-bias catalog with probes, a decision tree, and the shifting-bias-profile quantitative finding.

## Figure 1: Position Bias — The Reorder That Flips the Winner
Priority: Critical
Trigger: MC — the demonstration is a ≥3-step procedure (present A/B, present B/A, compare verdicts) whose dependency is the flip
Figure type: Comparison panels
Concept statement: Presenting the identical two answers to a judge in swapped order can flip the verdict, so a measured "winner" can be an artifact of position rather than quality.
Reader prior knowledge: Reader has the §8.1 Wang et al. demonstration; has not seen the swap-flips-verdict shown as paired panels.
Source anchor: Section: 8.1 The same two answers, reordered, flipped the winner

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector figure of two side-by-side panels sharing a central judge node. In the left panel, two candidate boxes feed the judge in one stacked order (first candidate on top), with a single-headed arrow from the judge to a verdict marker pointing at the top candidate, drawn in Bluish Green #009E73 for the winner and neutral light gray for the loser. In the right panel, the same two candidate boxes feed the same judge but with their vertical positions swapped, and the verdict marker now points at the now-top candidate, again Bluish Green #009E73 for the winner. Use the identical two candidate fill colors across both panels (one Blue #0072B2, one Orange #E69F00) so the reader sees the same answers changed only in order, yet the green winner-marker moves. Draw the judge node as a neutral gray hexagon centered between the panels or duplicated per panel. Keep to six components per panel. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Two panels, same two candidates (one Blue, one Orange) in swapped vertical order, a judge node per panel, a green winner-marker that moves with position. Preserve: identical candidates, only order changes, winner flips.
[O - Organization] Two side-by-side panels; candidates stacked vertically feeding a judge via single-headed arrows; verdict marker pointing at the winning candidate.
[P - Presentation] Flat vector, Okabe-Ito: candidate A Blue #0072B2, candidate B Orange #E69F00, winner-marker Bluish Green #009E73, judge node neutral light gray; 1pt strokes; no text in image.
[E - Exclusions] No "A"/"B" letters, no order numbers, no verdict text, no scores, no legend.

### Block 3 — Negative Prompt
candidate letters, order numbers, verdict text, scores, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Quality Gap Modulates Bias — Coarse vs. Close Call
Priority: Critical
Trigger: VG — the decision rule (trust on wide gaps, refuse near the margin) is a structural reliability claim not depictable from text
Figure type: Comparison panels
Concept statement: When two candidates differ widely in quality the judge's verdict tracks quality and position bias is small, but as the quality gap shrinks toward a near-tie the verdict is increasingly driven by ordering — so judges are reliable for coarse separation and untrustworthy at the margin.
Reader prior knowledge: Reader has the §8.3 Shi et al. quality-gap mechanism; has not seen the reliability-vs-gap relationship drawn.
Source anchor: Section: 8.3 The decision pivot: coarse ranking is fine, close calls are not

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector figure with two stacked panels each showing a horizontal quality axis with two candidate markers on it. In the top panel ("wide gap"), place the two candidate markers far apart on the axis, and draw a stable verdict zone around the clearly-higher marker shaded solid Bluish Green #009E73 to signal a trustworthy verdict. In the bottom panel ("near tie"), place the two markers almost coincident at the axis center, and draw an unstable verdict zone spanning both markers shaded Orange #E69F00 with a small bidirectional-uncertainty band (a single light gray oscillation glyph, not a dual-headed arrow) to signal the verdict is order-driven and unreliable. Keep the two axes aligned vertically so gap width is directly comparable. Limit to: two axes, four candidate markers, two verdict zones, one uncertainty glyph. Use 1pt strokes, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Two aligned quality axes; wide-gap pair with a stable green verdict zone; near-tie pair with an unstable orange verdict zone. Preserve: gap width drives verdict stability.
[O - Organization] Two stacked horizontal axes, candidate markers as dots, verdict zones as shaded bands around the predicted winner.
[P - Presentation] Flat vector, Okabe-Ito: trustworthy verdict Bluish Green #009E73, unreliable verdict Orange #E69F00, uncertainty glyph and axes neutral light gray, candidate dots Blue #0072B2; 1pt strokes; no text in image.
[E - Exclusions] No axis labels, no "wide gap"/"near tie" text, no numeric quality values, no dual-headed arrows, no legend.

### Block 3 — Negative Prompt
axis labels, gap labels, quality numbers, dual-headed arrows, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: The Four-Bias Probe Catalog
Priority: Important
Trigger: VG — a catalog mapping four biases each to a probe is an organizational/hierarchy claim
Figure type: Comparison panels
Concept statement: Four judge biases — position, verbosity, self-preference, style — each have a distinct probe (swap order, pad length, score self-vs-other, hold-content-vary-style) that produces a measurable flip or score-lift rate.
Reader prior knowledge: Reader has the §8.4 bias table; has not seen the four biases and their probes as parallel visual cards.
Source anchor: Section: 8.4 The bias catalog — and why you test for it instead of memorizing it

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector figure of four equal-size cells in a 2x2 grid, each cell a parallel diagram of one bias probe. Cell one (position): two stacked candidate boxes with a swap-loop glyph (a single curved single-headed arrow turning order) feeding a judge. Cell two (verbosity): two candidate boxes, one drawn taller/padded than the other, feeding a judge. Cell three (self-preference): two candidate boxes, one marked with a small self-origin glyph (a mirror dot), feeding a judge. Cell four (style): two candidate boxes with identical core blocks but different framing outlines, feeding a judge. In each cell draw a small output meter (a short horizontal magnitude bar) showing the measured rate, filled Orange #E69F00 for the probed-bias direction. Draw the judge node in each cell as a neutral gray hexagon and the candidate boxes in Blue #0072B2. Keep each cell to four components. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] 2x2 grid of probe diagrams: position (swap loop), verbosity (one padded box), self-preference (self-origin glyph), style (different outline). Each cell ends in a magnitude meter. Preserve: one distinct manipulation per bias plus a measured-rate readout.
[O - Organization] 2x2 grid, parallel internal layout per cell, candidates feeding a judge via single-headed arrows, a short horizontal meter as output.
[P - Presentation] Flat vector, Okabe-Ito: candidates Blue #0072B2, judge neutral gray, probed-bias meter Orange #E69F00; 1pt strokes; no text in image.
[E - Exclusions] No bias names, no probe names, no rate numbers, no cell titles, no legend.

### Block 3 — Negative Prompt
bias names, probe names, rate numbers, cell titles, legend, text labels, words, gibberish letters, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: Circularity — The Judge Shares the Generator's Blind Spot
Priority: Critical
Trigger: VG — circularity (evaluator is the same kind of thing as the generator) is a systems/structural claim
Figure type: Systems diagram
Concept statement: Because an LLM judge is the same kind of model as the generator, drawn from overlapping training data and objectives, a shared blind spot can make the judge approve a wrong-but-fluent answer precisely because the error is one the judge also holds.
Reader prior knowledge: Reader has the §8.5 self-recognition mechanism; has not seen the shared-blind-spot loop drawn.
Source anchor: Section: 8.5 Circularity: when the evaluator shares the generator's blind spot

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector systems diagram with a generator node on the left and a judge node on the right, both drawn as identical shapes filled the same Blue #0072B2 to signal same-kind-of-thing. Beneath both, draw one shared region (a common rounded band labeled by position as their overlapping training/objectives) tinted Orange #E69F00, with thin single-headed arrows rising from the shared region into both the generator and the judge to show common lineage. The generator emits an output node (carrying a small Vermillion #D55E00 fault wedge for a wrong-but-fluent answer) via a single-headed arrow to the judge; the judge emits an approval marker in Bluish Green #009E73 back toward the output, completing the circular validation. The key visual is that the same Orange shared region feeds both nodes, so the fault is invisible to the judge. Limit to: generator, judge, shared region, faulted output, approval marker. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Generator and judge as identical nodes, a shared training/objective region feeding both, a faulted output, an approval marker closing the loop. Preserve: common lineage + a fault the judge fails to flag.
[O - Organization] Generator left, judge right, shared region below feeding both upward; output flows generator-to-judge; approval flows judge-to-output, forming a circle.
[P - Presentation] Flat vector, Okabe-Ito: generator and judge both Blue #0072B2 (same kind), shared region Orange #E69F00, fault wedge Vermillion #D55E00, approval Bluish Green #009E73; 1pt strokes; no text in image.
[E - Exclusions] No node names, no "generator"/"judge" text, no "blind spot" caption, no legend, no dual-headed arrows.

### Block 3 — Negative Prompt
node names, generator/judge labels, blind-spot caption, legend, dual-headed arrows, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 5: Single Judge vs. Diverse Jury (PoLL)
Priority: Important
Trigger: VG — the jury-vs-judge structure (disjoint families, partial circularity break) is an organization claim
Figure type: Comparison panels
Concept statement: A panel of several smaller models from disjoint families partially breaks circularity by covering one another's blind spots, whereas a single large judge concentrates one model's bias — but the panel only partially breaks circularity if members share training lineage.
Reader prior knowledge: Reader has the §8.5 PoLL result; has not seen the single-vs-panel structure and the residual-shared-bias caveat drawn.
Source anchor: Section: 8.5 (Verga et al. PoLL)

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector figure of two side-by-side panels. Left panel ("single judge"): one large judge node in Blue #0072B2 with a single blind-spot region drawn around it as a solid Orange #E69F00 zone covering its whole verdict. Right panel ("diverse jury"): three smaller judge nodes in three distinct Okabe-Ito hues (Blue #0072B2, Bluish Green #009E73, Reddish Purple #CC79A7) each with its own small, differently-placed Orange #E69F00 blind-spot patch, feeding by single-headed arrows into one aggregation node; draw the aggregated coverage so the three patches mostly cover one another, leaving a small residual overlap patch where all three coincide (the surviving shared bias) tinted Orange #E69F00. The visual contrast: one big uncovered bias zone vs. a small residual shared zone. Limit to six nodes total across both panels plus the bias patches. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Single large judge with a full-coverage bias zone vs. three diverse small judges with offset bias patches feeding an aggregator, leaving a small residual shared patch. Preserve: diversity shrinks but does not erase shared bias.
[O - Organization] Two panels; left one node, right three nodes into an aggregator; bias patches as shaded zones; single-headed arrows.
[P - Presentation] Flat vector, Okabe-Ito: jury members Blue #0072B2 / Bluish Green #009E73 / Reddish Purple #CC79A7, all bias patches Orange #E69F00, aggregator neutral gray; 1pt strokes; no text in image.
[E - Exclusions] No model names, no "single judge"/"jury" text, no count labels, no legend, no red-green pairings.

### Block 3 — Negative Prompt
model names, panel titles, count labels, legend, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — position-bias flip: VIDEO CANDIDATE — the reorder-then-watch-the-winner-move is inherently a before/after action — animating the swap and the verdict jump is the cleanest possible demonstration.
FIGURE 2 — quality gap modulation: STATIC SUFFICIENT — two aligned axes read at a glance; a slider animation is optional but the static pair carries the rule.
FIGURE 3 — four-bias probe catalog: STATIC SUFFICIENT — a parallel reference grid is meant to be scanned, not played.
FIGURE 4 — circularity loop: STATIC SUFFICIENT — the shared-lineage loop is a fixed topology grasped statically; motion adds little.
FIGURE 5 — single vs. jury: STATIC SUFFICIENT — the coverage contrast is spatial, not temporal.
Recommendation: Build all five as static figures; promote only Figure 1 to a short animation, since the position-swap flip is the chapter's signature live demonstration and benefits most from motion.
