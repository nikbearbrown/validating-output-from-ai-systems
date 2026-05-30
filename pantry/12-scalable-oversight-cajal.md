# CAJAL Figure Intelligence — Chapter 12 — Scalable Oversight: The Unsolved Problem

Source: chapters/12-scalable-oversight.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
4 figure candidates. Mixed density. The chapter pairs one durable structural claim (the supervision-cost asymmetry that widens with capability) with a four-way comparison of partial methods, one process protocol (debate), and one quantitative degradation curve — each a distinct, non-overlapping figure.

## Figure 1: The Verification-Cost Asymmetry at the Frontier
Priority: Critical
Trigger: VG — the central claim is structural ("supervision cost rises until it crosses the budget and direct verification is no longer available"), a relationship not depictable from the prose alone
Figure type: Conceptual map
Concept statement: As model capability rises, the cost of a human directly verifying a given output rises with it, until that cost crosses a fixed overseer budget — past the crossing point, direct human verification is no longer available and scalable oversight is the search for methods that hold reliability beyond it.
Reader prior knowledge: Reader has the §12.1 review-you-cannot-do scene and the §12.2 supervision-cost framing; has not seen the cost curve crossing the budget line drawn against a capability axis.
Source anchor: Section: 12.2 The problem, stated stably

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector figure on a single two-axis field with a horizontal axis running left to right (increasing capability) and a vertical axis (cost / difficulty of direct human verification), both axes as plain 1pt lines with no tick text. Draw one rising curve from lower-left to upper-right in Vermillion #D55E00 representing the cost of a human directly verifying the output, curving steeply upward. Draw one flat horizontal line partway up the field in neutral light gray representing the fixed overseer budget. Mark the single point where the rising curve crosses the flat line with a small filled circle in Black #000000. Shade the region of the field to the left of that crossing in a light fill of Bluish Green #009E73 (direct verification still affordable / available) and the region to the right in a light fill of Vermillion #D55E00 (verification unaffordable / unavailable). Keep to: two axes, one rising curve, one budget line, one crossing marker, two shaded regions. Use 1pt strokes, no arrowheads on the curves, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] A two-axis field (capability horizontal, verification cost vertical), a steeply rising verification-cost curve, a flat horizontal overseer-budget line, the single crossing point, and the before/after regions split by it. Preserve: the curve crossing the fixed budget, and the qualitative shift from available to unavailable.
[O - Organization] Plain axes; rising curve from lower-left to upper-right; horizontal budget line; crossing marker; left region shaded affordable, right region shaded unavailable.
[P - Presentation] Flat vector, Okabe-Ito: verification-cost curve Vermillion #D55E00, budget line neutral light gray, crossing marker Black #000000, left region light Bluish Green #009E73 fill, right region light Vermillion #D55E00 fill; 1pt strokes; no text in image.
[E - Exclusions] No axis labels, no numbers, no tick marks with values, no legend, no "budget"/"capability" words, no equation.

### Block 3 — Negative Prompt
axis labels, numeric ticks, legend, budget/capability words, equations, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Four Partial Answers, One Shared Assumption
Priority: Critical
Trigger: VG — the §12.3 table is a structural taxonomy (four approaches, each with what-it-assumes / where-it-breaks, all resting on one shared bar) that organizes the chapter's argument
Figure type: Comparison panels
Concept statement: Four candidate replacements for the failing human oracle — debate, weak-to-strong, decomposition/recursive reward modeling, and PRMs — each rest on the same load-bearing assumption that judging is reliably easier than generating, recursively, so knocking out that single shared bar makes all four wobble at once.
Reader prior knowledge: Reader has the four prose treatments and the four-row table; has not seen the four approaches arranged as parallel panels standing on one common supporting bar.
Source anchor: Section: 12.3 Four partial answers, one shared assumption

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector figure with four equal-width vertical panels arranged side by side across the top, each panel an identical rounded rectangle representing one approach. Inside each of the four panels draw two stacked small zones: an upper zone marked with a short up-tick in Bluish Green #009E73 (the hope / what it assumes) and a lower zone marked with a short down-tick in Vermillion #D55E00 (where it breaks). Render the four panel outlines in Blue #0072B2 to read as parallel siblings. Beneath all four panels, draw one single wide horizontal bar spanning the full width in Orange #E69F00, positioned so all four panels rest on top of it — this shared bar is the common assumption. Draw four short single-headed downward arrows, one from the bottom of each panel onto the shared bar, in neutral light gray, showing all four depend on it. Keep to: four panels, each with an up-zone and down-zone, one shared base bar, four dependency arrows. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Four parallel approach panels, each split into a hope zone and a breaks zone; one shared horizontal base bar all four rest on; dependency arrows from each panel to the bar. Preserve: four siblings, the per-panel hope/breaks split, and the single common foundation.
[O - Organization] Four equal panels across the top; one full-width bar beneath; four short downward arrows linking panels to bar.
[P - Presentation] Flat vector, Okabe-Ito: panel outlines Blue #0072B2, hope up-ticks Bluish Green #009E73, breaks down-ticks Vermillion #D55E00, shared base bar Orange #E69F00, dependency arrows neutral light gray; 1pt strokes; no text in image.
[E - Exclusions] No approach names, no "hope"/"breaks" words, no table cells with text, no legend, no numeric scores.

### Block 3 — Negative Prompt
approach names, hope/breaks words, table text, legend, numeric scores, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: The Debate Protocol — Two Arguers, One Weaker Judge
Priority: Important
Trigger: MC — the debate protocol is a multi-step interaction (two AIs assert opposing answers → exchange rebuttals exposing each other's errors → weaker judge reads the exchange → judge picks the more truthful)
Figure type: Systems diagram
Concept statement: In debate, two AI debaters argue opposing answers and rebut each other so that each can expose the other's errors, and a weaker judge — who may not see the underlying evidence — decides between them, the claim being that judging an adversarial exchange is easier than judging an answer cold.
Reader prior knowledge: Reader has the §12.3 debate prose and the PSPACE-vs-NP framing; has not seen the two-debater / one-judge information flow drawn, including the judge's restricted view.
Source anchor: Section: 12.3 Four partial answers, one shared assumption (Debate)

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector systems diagram. Place two debater nodes on the left, stacked one above the other: the top debater node in Blue #0072B2 (arguing answer A) and the bottom debater node in Orange #E69F00 (arguing answer B). Draw two single-headed rebuttal arrows between them — one from the top node curving to the bottom node and one from the bottom node curving to the top node — both in neutral light gray, showing the adversarial exchange. From each debater node draw one single-headed arrow rightward into a single judge node on the right, rendered as a smaller circle in Bluish Green #009E73 to signal it is the weaker overseer. To the far left, draw a hidden-evidence box in neutral light gray connected by single-headed arrows to both debaters but NOT to the judge, depicting the information asymmetry (the judge cannot see the passage). From the judge node draw one single-headed arrow to a small verdict marker on the far right in Black #000000. Keep to: two debater nodes, one evidence box, two rebuttal arrows, two debater-to-judge arrows, one judge node, one verdict marker. Use 1pt strokes, single-headed arrows only, white background, no text anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] Two opposing debater nodes with reciprocal rebuttal arrows, a hidden-evidence box feeding only the debaters, a smaller weaker-judge node receiving from both debaters, and a verdict marker. Preserve: the adversarial exchange, the judge's restricted view (asymmetry), and the judge being the smaller/weaker node.
[O - Organization] Debaters left (stacked), evidence box far left feeding debaters only, judge right, verdict far right.
[P - Presentation] Flat vector, Okabe-Ito: debater A Blue #0072B2, debater B Orange #E69F00, judge Bluish Green #009E73, rebuttal and evidence-feed arrows neutral light gray, verdict marker Black #000000; 1pt strokes; single-headed arrows only; no text in image.
[E - Exclusions] No "debater"/"judge"/"answer A" words, no speech bubbles, no PSPACE/NP text, no legend, no scores.

### Block 3 — Negative Prompt
debater/judge/answer words, speech bubbles, complexity-class text, legend, scores, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: Nested-Oversight Success vs. Capability Gap
Priority: Important
Trigger: PQ — the Engels et al. result is a magnitude (nested oversight success drops below ~52% at a ~400 Elo gap and declines further), a quantity warranting a chart with y-axis from zero
Figure type: Statistical/quantitative
Concept statement: Modeling oversight as a game between capability-mismatched players, nested scalable-oversight success declines as the overseen system's Elo advantage grows, falling below roughly 52% at a ~400 Elo gap — a sobering downward trend, presented as one study's numerical example rather than a validated law.
Reader prior knowledge: Reader has the §12.3 Engels et al. figure and its explicit "one study, a few games" hedge; has not seen the success rate plotted against the capability gap.
Source anchor: Section: 12.3 Four partial answers, one shared assumption (Engels et al. degradation)

### Block 1 — Illustrae Paste Block
Create a blank, unannotated flat vector line chart. Draw a horizontal axis (capability gap, increasing left to right) and a vertical axis (oversight success rate) as plain 1pt black lines, the vertical axis beginning at zero at the origin so the full magnitude is visible. Plot one descending curve from upper-left to lower-right in Blue #0072B2, starting high near the top of the vertical range and sloping downward as the gap widens. Draw a single horizontal reference line at the mid-height (the ~52% level) in neutral light gray, dashed-equivalent rendered as a thin solid 1pt line. Mark the point where the descending curve crosses that reference line with a small filled circle in Vermillion #D55E00 (the ~400-Elo crossing). Keep to: two axes from zero, one descending curve, one horizontal reference line, one crossing marker. Use 1pt strokes, no arrowheads, white background, no text or numbers anywhere. Flat vector, Okabe-Ito palette only.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector, white background, unannotated.
[C - Content] A two-axis chart with the vertical (success) axis starting at zero, one descending success curve, a horizontal mid-level reference line, and the crossing point marked. Preserve: the downward trend with widening gap and the single notable crossing.
[O - Organization] Capability gap on the horizontal axis, success rate on the vertical from zero; descending curve; horizontal reference line; crossing marker.
[P - Presentation] Flat vector, Okabe-Ito: success curve Blue #0072B2, reference line neutral light gray, crossing marker Vermillion #D55E00, axes Black #000000; 1pt strokes; no text in image.
[E - Exclusions] No numeric axis values, no "52%"/"400 Elo" text, no gridlines, no legend, no title.

### Block 3 — Negative Prompt
numeric axis values, percentage labels, Elo labels, gridlines, legend, title, text labels, words, gibberish letters, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — verification-cost asymmetry: STATIC SUFFICIENT — single static relationship (one curve crossing one line) — the crossing is the whole point and reads in one frame; no temporal unfolding needed.
FIGURE 2 — four answers, one shared bar: STATIC SUFFICIENT — taxonomy/comparison — the parallel structure and common foundation are spatial, fully legible at rest.
FIGURE 3 — debate protocol: VIDEO CANDIDATE — multi-step interaction with an information-flow sequence (assert → rebut → judge reads → verdict) — a short build that reveals the rebuttal exchange then the judge's restricted view would clarify the asymmetry, though the static systems diagram is adequate.
FIGURE 4 — nested-oversight curve: STATIC SUFFICIENT — single quantitative trend — one descending line reads fully as a static chart.

Recommendation: Produce all four as static figures; Figure 1 and Figure 2 are Critical and should be drawn first. Optionally animate Figure 3 as a brief sequenced build if a motion asset is wanted, but the static version stands alone.
