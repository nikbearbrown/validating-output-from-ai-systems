# CAJAL Figure Intelligence — Chapter 10 — Human Validation and Automation Bias

Source: chapters/10-human-validation-and-automation-bias.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
4 figure candidates. Mixed density. The chapter pairs a mechanistic core (automation-bias anchoring, the four-lever design) with one quantitative tension (forcing-function effectiveness vs. reviewer preference) and one comparison workflow, warranting a small set of high-yield figures rather than illustration of every section.

## Figure 1: Forcing-function review workflow
Priority: Critical
Trigger: MC — process of ≥3 interdependent, ordered steps (decide-before-seeing → delayed reveal → adversarial prompt → bounded scope → risk-gated approval)
Figure type: Process flowchart
Concept statement: The engineered human-review workflow sequences five interventions so a reviewer commits independent judgment before the fluent AI output can anchor them, then routes only rare, consequential actions to a human gate.
Reader prior knowledge: They know reviewing and approving exist; they have not seen the four levers assembled into one ordered, anchoring-defeating sequence with a risk-proportional terminal gate.
Source anchor: Section: 10.4 The four levers: designing the human layer

### Block 1 — Illustrae Paste Block
Create a single-column blank unannotated flat vector process flowchart on a white background for a textbook, showing the forcing-function review workflow as a left-to-right linear chain of five rounded-rectangle stage nodes connected by single-headed arrows. Stage one (reviewer commits independent judgment) in bluish green #009E73 to mark the active anchoring-defeating move; stage two (delayed reveal of AI output with partial explanation) in sky blue #56B4E9; stage three (adversarial prompt) in blue #0072B2; stage four (bounded scope, machine-catchable errors pre-filtered) in orange #E69F00 as a secondary support step; stage five drawn as a decision diamond (risk-proportional gate) in sky blue #56B4E9, branching to two terminal nodes — a batched/auto-approve outcome in light gray and a human-gate outcome for rare consequential actions in bluish green #009E73. Use no more than eight components total, 1pt strokes, consistent single-headed arrows, even spacing, flat fills, no shadows or gradients. Leave every node blank with no text, words, titles, or captions. Map flow strictly left to right with the single branch only at the diamond.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Five sequential stages — independent-judgment commit, delayed partial-explanation reveal, adversarial prompt, bounded scope, risk-proportional gate — plus two terminal outcomes (batched/auto-approve; human gate). Preserve strict ordering and the single branch at the gate.
[O - Organization] Left-to-right linear chain of four rounded rectangles into one decision diamond; diamond branches downward/rightward to two terminal nodes; single-headed arrows only.
[P - Presentation] Flat vector, Okabe-Ito: #009E73 (active commit + human-gate terminal), #56B4E9 (reveal + diamond), #0072B2 (adversarial), #E69F00 (bounded scope), light gray (auto-approve terminal); 1pt strokes; no text in image.
[E - Exclusions] No text labels, numbers, legends, icons of people, percentages, or backgrounds; no second branch anywhere except the diamond; no color outside the listed hexes.

### Block 3 — Negative Prompt
people icons, approval buttons, screenshots, branch points other than the single decision diamond, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Automation-bias anchoring mechanism
Priority: Important
Trigger: VG — structural/causal claim ("fluent output anchors the reviewer, reducing vigilance via cognitive load") not depictable as a plain list
Figure type: Systems diagram
Concept statement: Fluent AI output anchors the reviewer and raises cognitive load, which suppresses independent search and produces omission errors — the loop a forcing function interrupts at the anchoring step.
Reader prior knowledge: They know reviewers sometimes miss errors; they have not seen anchoring, cognitive load, and reduced vigilance drawn as a single causal pathway with the intervention point marked.
Source anchor: Section: 10.2 The mechanism: what automation bias is, and what mediates it

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector systems diagram on a white background for a single-column textbook, showing a left-to-right causal chain of five nodes connected by single-headed arrows. Node one (fluent AI output revealed first) in sky blue #56B4E9; node two (reviewer anchored on the fluent answer) in orange #E69F00; node three (elevated cognitive load / verification complexity) in orange #E69F00; node four (reduced independent search / vigilance) in vermillion #D55E00 to mark the harmful state; node five (omission error reaches production) in vermillion #D55E00. Add one separate intervention element — a small blocking bar in bluish green #009E73 drawn cutting the arrow between node one and node two, marking where a cognitive forcing function breaks anchoring. Use at most seven components, 1pt strokes, flat fills, consistent single-headed arrows, even horizontal spacing. Leave all nodes and the intervention element completely blank — no text, words, numbers, titles, or captions. Avoid shadows, gradients, and any decorative framing.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Five-node causal chain (fluent output → anchoring → elevated cognitive load → reduced vigilance → omission error) plus one intervention bar interrupting the first arrow. Preserve the causal direction and the placement of the interrupt at the anchoring step.
[O - Organization] Horizontal left-to-right chain; single-headed arrows between nodes; one perpendicular blocking bar across the first arrow.
[P - Presentation] Flat vector, Okabe-Ito: #56B4E9 (input), #E69F00 (anchoring + load), #D55E00 (reduced vigilance + omission error), #009E73 (intervention bar); 1pt strokes; no text in image.
[E - Exclusions] No text, no brains/heads/people imagery, no percentages, no feedback loops not stated in source, no color outside listed hexes, no second intervention point.

### Block 3 — Negative Prompt
brain icons, head silhouettes, eye icons, feedback loop arrows not specified, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: The four-lever review-design map
Priority: Important
Trigger: VG — a structural "how the mitigations are organized" claim (four levers, each mapped to a distinct cognitive-load mechanism) not self-evident from prose
Figure type: Conceptual map
Concept statement: Four designable levers — cognitive forcing functions, adversarial framing, bounded scope, fatigue-aware approval — each act on the cognitive-load mediator from a different angle to convert the human layer from exhortation into engineering.
Reader prior knowledge: They have met the levers individually in the text; they have not seen the four held together as a single coordinated map around the shared cognitive-load target.
Source anchor: Section: 10.4 The four levers: designing the human layer

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector conceptual map on a white background for a single-column textbook, showing one central hub node with four satellite nodes arranged symmetrically around it, each connected to the hub by a single straight line. The central hub (the cognitive-load mediator / the human layer target) in light gray as the neutral anchor. The four satellites: forcing functions in bluish green #009E73, adversarial framing in blue #0072B2, bounded scope in sky blue #56B4E9, fatigue-aware approval in orange #E69F00. Optionally include a small fifth satellite (disposition-aware staffing) in reddish purple #CC79A7, drawn with a dashed connector to mark it as the inferred organizational lever rather than a core mechanism. Use at most six components, 1pt strokes, flat fills, even radial spacing, equal-length connectors. Leave every node blank — no text, words, numbers, titles, or captions. Avoid shadows, gradients, icons, and decorative framing.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] One central hub (cognitive-load target) with four core satellite levers and one optional fifth (staffing) on a dashed connector. Preserve the hub-and-spoke relationship; the dashed link marks the inferred/secondary lever.
[O - Organization] Radial hub-and-spoke; four solid equal-length spokes plus one dashed spoke; symmetric placement.
[P - Presentation] Flat vector, Okabe-Ito: light gray (hub), #009E73 (forcing functions), #0072B2 (adversarial), #56B4E9 (bounded scope), #E69F00 (fatigue-aware approval), #CC79A7 dashed (staffing); 1pt strokes; no text in image.
[E - Exclusions] No text, no icons inside nodes, no arrowheads on spokes, no hierarchy implied beyond hub-and-spoke, no color outside listed hexes.

### Block 3 — Negative Prompt
icons inside nodes, arrowheads on spokes, nested rings, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: Fatigue-aware vs. blanket approval
Priority: Supplementary
Trigger: PQ — magnitude/distribution contrast (many low-stakes prompts vs. few high-stakes prompts) plus a comparison of two approval regimes
Figure type: Comparison panels
Concept statement: Blanket approval forces a human to act on a high volume of mostly-benign decisions (manufacturing fatigue), whereas risk-proportional approval batches or auto-approves the benign and surfaces only the rare consequential actions to the human.
Reader prior knowledge: They understand approval volume drives rubber-stamping; they have not seen the two regimes contrasted as distributions of where human attention is spent.
Source anchor: Section: 10.4 The four levers: designing the human layer (Lever 4)

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector two-panel comparison figure on a white background for a single-column textbook, panels stacked or side by side and identically sized. Left/top panel (blanket approval): a single column of many small uniform squares, all the same vermillion #D55E00, representing a large undifferentiated volume of human-routed approvals. Right/bottom panel (risk-proportional approval): the same total quantity of squares split into two groups — a large block of light-gray squares (batched/auto-approved, off the human path) and a small cluster of bluish green #009E73 squares (the few consequential actions surfaced to a human). Keep the total square count visually equal between panels so the redistribution reads clearly. Use a simple thin divider or equal frames to separate panels. Use 1pt strokes, flat fills, aligned grid spacing, at most two panels and three color groups. Leave the figure entirely blank — no text, words, numbers, axis ticks, titles, or captions. No shadows, gradients, or decoration.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Two equally sized panels comparing approval regimes; identical total unit count; panel one all-human-routed, panel two split into mostly auto-approved plus a small human-surfaced subset. Preserve equal totals so redistribution is the visible message.
[O - Organization] Two aligned panels (stacked or side by side) with matched grids; thin divider or equal frames.
[P - Presentation] Flat vector, Okabe-Ito: #D55E00 (human-routed volume), light gray (auto-approved/batched), #009E73 (consequential human-surfaced subset); 1pt strokes; no text in image.
[E - Exclusions] No axis labels, numbers, legends, icons, or people; no unequal panel sizes; no color outside listed hexes; no implied baseline other than zero for the grid.

### Block 3 — Negative Prompt
axis numbers, legends, person icons, unequal panel sizes, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — forcing-function workflow: VIDEO CANDIDATE — sequential staged process with a branch — a short build-up animation revealing one stage at a time would reinforce the "commit before reveal" ordering, but the static flowchart fully conveys it.
FIGURE 2 — anchoring mechanism: STATIC SUFFICIENT — single causal pathway with one interrupt — the whole claim is legible in one frame; motion adds nothing.
FIGURE 3 — four-lever map: STATIC SUFFICIENT — fixed hub-and-spoke relationship — no temporal or process dimension to animate.
FIGURE 4 — fatigue-aware comparison: STATIC SUFFICIENT — two-state distribution contrast — the before/after redistribution reads at a glance in a panel pair.

Recommendation: Build all four as static figures; Figure 1 (Critical) is the chapter's spine. Reserve any animation budget for Figure 1 only if the workflow's "commit-before-reveal" ordering tests poorly as a static chain.
