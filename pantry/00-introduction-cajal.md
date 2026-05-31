# CAJAL Figure Intelligence — Introduction — Validating Output from AI Systems

Source: chapters/00-introduction.md
Mode: /scan silent
Date: 2026-05-30

## Density Recommendation
1 figure candidate. Foundational density. The introduction is almost entirely prose establishing a stance; its one figure-worthy zone is the book's recurring concept — the boundary between *execution* and *judgment* — which is asserted structurally in text but never depicted. The "How This Book Is Organized" section is a 13-item navigational list (a table of contents), not a figure: it exceeds the 6–8 component limit and carries no mechanism, verification gap, or quantitative claim. It is correctly left to the prose. No quantitative data appears (the "14 body chapters" count is metadata, not a chart).

## Figure 1: The Execution–Judgment Boundary
Priority: Important
Trigger: VG — a structural/conceptual distinction asserted in prose ("the boundary between execution and judgment") that the reader is asked to hold for the entire book but which the text never grounds visually; the judgment side decomposes into a fixed, enumerable set of questions that reward side-by-side display against the single execution claim.
Figure type: Comparison panels (two domains across a shared dividing boundary)
Concept statement: Execution is the cheap production of an artifact; judgment is the disciplined, non-delegable decision about whether that artifact should exist, whether it is right, whether it fits the situation, what it omits, and what consequences follow from using it.
Reader prior knowledge: A reader at the very start of the book. Has the everyday words "execution" and "judgment" but has not yet seen them drawn as a boundary, and has not seen the judgment side broken into its five questions. Both sides must be shown; nothing can be assumed structural.
Source anchor: Sections "The Recurring Concept" and "Closing Return"

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector comparison figure, single column, white background, two panels separated by one strong vertical dividing line down the center. The left panel holds a single anchor box representing the production of an artifact (one box only — drafting, summarizing, generating, transforming collapse into this one cheap activity). The right panel holds a vertical stack of five small uniform boxes, each one a distinct judgment question (should it exist; is it right; does it fit the situation; what does it omit; what consequences follow). Draw a single horizontal arrow crossing the center divider from left to right, indicating that an artifact moves from execution into judgment before it can be trusted. Color the left anchor box in the primary structural blue, the five right-side judgment boxes in bluish green, the dividing line and arrow in black. Keep exactly one box on the left and five on the right — seven components including the divider and arrow. Flat 1pt strokes, square corners, no shadows, no gradients, no text.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated. University-press / general-textbook default style.
[C - Content] One left-panel anchor box = "Execution: produce the artifact." One central vertical divider = the boundary. One left-to-right arrow crossing the divider = "must pass through judgment to be trusted." Five right-panel boxes = the five judgment questions: (1) should it exist, (2) is it right, (3) does it fit the situation, (4) what does it omit, (5) what consequences follow. Preserve: execution is one cheap activity; judgment is five irreducible questions; the artifact crosses the boundary in one direction.
[O - Organization] Two side-by-side panels split by a single central vertical line. Left: one anchor box, vertically centered. Right: five equal boxes stacked vertically. One horizontal arrow from the left box across the divider into the right stack.
[P - Presentation] Flat vector, Okabe-Ito mapped to the house palette: primary structural anchor (execution box) = blue; the five judgment boxes = bluish green (active, non-delegable competence); divider and arrow = ink/black. 1pt strokes, square corners, no text in image.
[E - Exclusions] No table of contents / chapter list. No depiction of specific AI tools, models, or interfaces. No "Medhavy/Medhavi" branding. No icons for drafting/coding/design (execution stays one abstract box). No value judgments rendered as red/green "good vs bad" — both sides are legitimate; the point is sequence, not approval. No third "automation" panel. No human figures.

### Block 3 — Negative Prompt
chapter list, table of contents, numbered chapter boxes, tool logos, model names, app screenshots, icons, human figures, checkmarks, x-marks, good-vs-bad coloring, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, drop shadows, gradient backgrounds, photographic elements, dual-headed arrows, hand-drawn styles, sketch lines, visual clutter, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

## Video Candidate Pass
Figure 1 — The Execution–Judgment Boundary
Status: STATIC SUFFICIENT
Criterion met: none. The figure shows a relationship between two states, not a transition mechanism the reader must mentally simulate. The crossing arrow is a single directional claim, not a multi-stage process. Static side-by-side panels allow self-paced inspection of the five judgment questions.

Video candidates identified: 0. Recommended for production: none. The introduction's single figure is well-served by static treatment.

## Notes
- Editorial flag (out of figure scope): the "How This Book Is Organized" list contains a stale/duplicate entry — "Chapter 1: Introduction" followed by "Chapter 1: Why Validating AI Output Is a Different Problem" (two Chapter 1s). This is the residue of the removed `01-introduction.md` stub and should be cleaned in the prose, not the figure.
