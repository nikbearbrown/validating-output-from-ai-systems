# CAJAL Figure Intelligence — Chapter 1 — Why Validating AI Output Is a Different Problem

Source: chapters/01-why-validating-ai-output-is-different.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
4 figure candidates. Mixed density. The chapter is conceptual but carries one hard quantitative result (compounding error), one structural/hierarchical claim (the responsibility stack), and two multi-component mechanisms (generate-verify asymmetry break, fluency-defeats-reviewer) that are not depictable from prose alone.

## Figure 1: The asymmetry breaks when the checker is not independent
Priority: Critical
Trigger: VG — "the asymmetry assumes the verifier is independent of the generator," a structural claim about where the check sits relative to the generator
Figure type: Comparison panels
Concept statement: Verification is cheap only when the checker is a different mechanism from the generator; a second LLM pass is the same machine, so it is not an independent check.
Reader prior knowledge: Reader knows the Sudoku generate-vs-verify intuition and that compilers check cheaply; they have not seen *why* a second LLM pass fails to inherit that cheapness — that it shares the generator's training distribution and blind spots.
Source anchor: Section: 1.2 The asymmetry — and why it partly fails for LLMs

### Block 1 — Illustrae Paste Block
Create a two-panel comparison. Left panel: a generator node feeding a solution into a distinct, separate checker node of a clearly different internal structure, with a single arrow from generator to checker and a second arrow from checker to a verdict marker — the independent-check case. Right panel: a generator node feeding a solution into a second node that is visually identical in internal structure to the generator (same shape, same fill), connected by a single arrow, with a loop-back arrow indicating the second pass is conditioned on the first — the non-independent case. Use Blue (#0072B2) for the generator nodes as the dominant anchor; use Bluish Green (#009E73) for the independent checker (active, trustworthy) in the left panel; use Vermillion (#D55E00) for the non-independent second pass (blocking/unreliable) in the right panel; verdict markers in light gray. Keep to six total components. Render as a blank, unannotated, flat vector on a white background with uniform single-weight strokes and no baked-in text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated.
[C - Content] left panel: generator node, distinct checker node, verdict marker; right panel: generator node, second pass node identical to generator, verdict marker. The relationship to preserve: structural sameness vs. difference between generator and checker.
[O - Organization] two side-by-side panels of equal size; within each panel left-to-right flow generator → checker → verdict; one loop-back arrow in the right panel only.
[P - Presentation] flat vector, Okabe-Ito mapping — generator #0072B2, independent checker #009E73, non-independent second pass #D55E00, verdict markers light gray; uniform 1pt strokes; no text labels in image.
[E - Exclusions] no Sudoku grid imagery; no compiler/CI iconography (that is Chapter 2); no training-data or distribution clouds; no human reviewer figure; no NP/complexity-class notation; no fluency or automation-bias elements.

### Block 3 — Negative Prompt
sudoku grids, compiler icons, brain imagery, training data clouds, human figures, complexity notation, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Fluency suppresses scrutiny exactly where it should rise
Priority: Important
Trigger: MC — a three-element interaction (task complexity → fluency/verification difficulty → reviewer deference) where the trap "snaps shut" only when all three combine
Figure type: Systems diagram
Concept statement: As task complexity rises, AI output becomes more fluent and harder to independently verify, and automation bias rises with verification difficulty — so deference is highest precisely where the output is least checkable.
Reader prior knowledge: Reader knows automation bias exists and that fluency is a surface property; they have not seen the *reinforcing* relationship — that the same axis (complexity) drives both harder-to-verify output and higher human deference, closing the trap.
Source anchor: Section: 1.3 The fluency problem — "Fluency does not just fool you — it lowers your guard"

### Block 1 — Illustrae Paste Block
Create a systems diagram with a single driving node on the left (task complexity) and two parallel consequence nodes to its right — one for verification difficulty rising, one for reviewer deference rising — each reached by its own single arrow from the driver. Then add a reinforcing arrow from the verification-difficulty node into the deference node, showing they compound. Terminate in a single failure-outcome node on the far right that both consequence nodes feed into. Use Blue (#0072B2) for the driving complexity node as the primary anchor; Orange (#E69F00) for the two intermediate consequence nodes as secondary; Vermillion (#D55E00) for the final failure-outcome node as the blocking/negative endpoint; reinforcing arrow in neutral light gray. Keep to five components and four arrows. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated.
[C - Content] driver node (complexity), two parallel consequence nodes (verification difficulty, deference), one reinforcing link between them, one terminal failure node. Relationship to preserve: one driver fans out to two consequences that mutually reinforce and converge on failure.
[O - Organization] left-to-right; driver on far left, two consequence nodes stacked vertically in the middle, failure node on far right; single-headed arrows only; one horizontal reinforcing arrow between the two middle nodes.
[P - Presentation] flat vector, Okabe-Ito mapping — driver #0072B2, consequences #E69F00, failure #D55E00, reinforcing link light gray; uniform 1pt strokes; no text labels in image.
[E - Exclusions] no aircraft/aviation imagery; no statistics or effect-size numbers; no generate-verify panels (that is Figure 1); no compounding-error chain; no responsibility-stack layers; no citation or code examples.

### Block 3 — Negative Prompt
aircraft, cockpit instruments, percentage figures, effect sizes, code snippets, citation imagery, stack/layer diagrams, chains, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: Compounding error — end-to-end reliability decays with chain length
Priority: Critical
Trigger: PQ — explicit magnitudes: 0.9^10 ≈ 0.35, 0.9^20 ≈ 0.12, per-step 90% reliability
Figure type: Statistical/quantitative
Concept statement: When each step is 90% reliable and errors propagate, end-to-end success decays exponentially with the number of steps — from ~35% at ten steps to ~12% at twenty.
Reader prior knowledge: Reader can do the arithmetic but has not *seen* the exponential decay curve; the figure makes the gap between per-step reliability (flat 90%) and task reliability (collapsing) visible.
Source anchor: Section: 1.4 Compounding error — how a 90%-reliable agent fails a task

### Block 1 — Illustrae Paste Block
Create a single line chart with a y-axis starting at zero and running to 1.0 (end-to-end success probability) and an x-axis for number of steps from 1 to 20. Plot one decaying curve following 0.9 raised to the step count, starting near 0.9 at one step and falling to roughly 0.12 at twenty steps. Add a second flat horizontal reference line at 0.9 representing constant per-step reliability, so the widening gap between the flat line and the decaying curve is the visual point. Mark two emphasis points on the decaying curve at step 10 and step 20. Use Blue (#0072B2) for the decaying end-to-end curve as the dominant anchor; use neutral light gray for the flat per-step reference line; use Orange (#E69F00) for the two emphasis point markers. The y-axis must begin at zero. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in numeric labels or axis text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated, y-axis from zero.
[C - Content] one exponential-decay curve (0.9^n), one flat horizontal reference line at the per-step value, two marked points at n=10 and n=20, plain x and y axes. Relationship to preserve: exponential decay diverging from a flat reference.
[O - Organization] standard Cartesian plot, x-axis horizontal (steps 1–20), y-axis vertical from 0 to 1; curve descends left to right.
[P - Presentation] flat vector, Okabe-Ito mapping — decay curve #0072B2, reference line light gray, emphasis points #E69F00; uniform 1pt strokes; no text labels, no numeric tick labels in image.
[E - Exclusions] no agent/robot iconography; no step-node flowchart; no annotation callouts; no second curve other than the flat reference; no irreversible-action imagery; no trajectory diagram.
This block is the highest-leverage part: the y-axis MUST start at zero and only the single 0.9^n curve plus one flat reference line may appear.

### Block 3 — Negative Prompt
robot icons, agent avatars, flowchart nodes, multiple decay curves, annotation callouts, trajectory arrows, bar charts, text labels, words, gibberish letters, titles, captions, numeric tick labels, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion, truncated y-axis

---

## Figure 4: The validation-responsibility stack
Priority: Critical
Trigger: VG — a four-layer hierarchy ordered by cost and trustworthiness, with a "push the check down" directionality not visible in prose
Figure type: Hierarchy/taxonomy
Concept statement: Validation checks live in four ordered layers — model, application, infrastructure, human — and the discipline is to push each check down to the cheapest layer that can carry it.
Reader prior knowledge: Reader has met each layer individually; they have not seen the four arranged as a stack with the cost/trust gradient and the downward "push the check here" direction that is the chapter's design principle.
Source anchor: Section: 1.5 Where does the check live? The validation-responsibility stack

### Block 1 — Illustrae Paste Block
Create a vertical stack of four horizontal bands, evenly sized, arranged top to bottom: model/generation on top, then application, then infrastructure/CI, then human reviewer at the bottom — OR inverted if the layout reads cleaner, but keep a single consistent vertical order. Add a single vertical arrow alongside the stack indicating the "push the check down toward cheaper, more trustworthy" direction. Use a deliberate color gradient of *semantic*, not decorative, kind: the human layer (most expensive, most bias-prone) in Vermillion (#D55E00); the infrastructure/CI layer (cheap, exact, judgment-free) in Bluish Green (#009E73); the application layer in Orange (#E69F00); the model layer (cheapest to invoke, least trustworthy alone) in light gray. The directional arrow in Blue (#0072B2). Exactly four bands plus one arrow — five components. Render as a blank, unannotated, flat vector on a white background, uniform single-weight strokes, no baked-in text.

### Block 2 — Full SCOPE Prompt
[S - Specification] single-column 89mm textbook width, 300 DPI, vector preferred, white background, unannotated.
[C - Content] four stacked horizontal bands (model, application, infrastructure, human) and one directional arrow indicating where to push the check. Relationship to preserve: ordered layering plus a single push-the-check direction.
[O - Organization] vertical stack, four equal bands, one vertical directional arrow on the side; no cross-links between bands.
[P - Presentation] flat vector, Okabe-Ito mapping — human #D55E00, infrastructure #009E73, application #E69F00, model light gray, directional arrow #0072B2; uniform 1pt strokes; no text labels in image.
[E - Exclusions] no example tools inside the bands (no compiler/linter icons); no the four-column table; no automation-bias or fluency elements; no compounding-error curve; no generate-verify panels; no per-layer "catch/catch-not" annotations.
This block is the highest-leverage part: exactly four bands, no tool icons, no text inside bands.

### Block 3 — Negative Prompt
tool icons, compiler symbols, linter marks, gears, tables, grid cells, code snippets, annotation text, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — asymmetry break: STATIC SUFFICIENT — a side-by-side structural comparison reads in one glance; no temporal change to animate.
FIGURE 2 — fluency suppresses scrutiny: STATIC SUFFICIENT — the reinforcing-loop systems diagram is a single steady-state relationship, not a process unfolding over time.
FIGURE 3 — compounding error: VIDEO CANDIDATE — progressive build criterion — a short animation that adds one step at a time and shows the success bar shrinking from 0.9 toward 0.12 would make the exponential collapse felt rather than merely read, though the static curve already carries it.
FIGURE 4 — responsibility stack: STATIC SUFFICIENT — a hierarchy with a directional arrow is inherently static; the "push down" reads from the arrow alone.

Video candidates: 1. Recommended for production: none — all are well-served by static; Figure 3's exponential decay is the only one where animation adds genuine felt understanding, but the zero-based static curve is sufficient and cheaper, so production is optional rather than recommended.
