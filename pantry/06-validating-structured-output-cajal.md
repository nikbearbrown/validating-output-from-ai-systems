# CAJAL Figure Intelligence — Chapter 6 — Validating Structured Output

Source: chapters/06-validating-structured-output.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
4 figure candidates. Mechanistic density. The chapter turns on a two-gate pipeline, a token-masking mechanism, a spectrum of mechanizability, and a retry-vs-enforcement distinction — all structural and better shown than told.

## Figure 1: Two Gates — Structural (Solved) then Semantic (Thinning Ground Truth)
Priority: Critical
Trigger: MC + VG — a ≥2-stage interdependent pipeline where Gate 1 is hard-edged deterministic and Gate 2 is mixed/dashed; the structural separation is the chapter's spine and not depictable from prose
Figure type: Process flowchart
Concept statement: Structured-output validation is two physically separate gates — Gate 1 (structural validity) is a solved deterministic check; Gate 2 (semantic validity) is where ground truth thins out — and conflating them is how a 99.7% pass rate ships a wrong invoice.
Reader prior knowledge: Reader has the invoice failure story (§6.1); has not seen the two gates drawn as distinct stages with different edge-hardness encoding their different guarantees.
Source anchor: Section: 6.4 The two-gate pipeline, and the retry trap

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector process flowchart, single column, white background, flowing left to right. Start with a raw model-output node in light gray. An arrow leads into Gate 1, drawn as a solid-edged hard rectangle in blue #0072B2 (structural / deterministic), with a vermillion #D55E00 reject arrow exiting downward and a green #009E73 pass arrow continuing right. The pass arrow enters Gate 2, drawn as a dashed-edged rectangle in orange #E69F00 (semantic / mixed), with a vermillion #D55E00 reject arrow exiting downward and a green #009E73 accept arrow exiting right to a final accepted-record terminal in bluish green #009E73. Inside Gate 2, show a small internal gradient cue: a sub-region shaded solid (mechanizable checks) fading to a dashed sub-region (non-mechanizable truth). Keep exactly: input, Gate 1, Gate 2 (with internal split), two reject arrows, terminal — at most seven components. Solid edge = enforcement-strength; dashed edge = thinning ground truth. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Raw output; Gate 1 (structural, solid, deterministic); Gate 2 (semantic, dashed, mixed mechanizability with an internal solid→dashed split); two reject exits; accepted terminal. Preserve: Gate 1 precedes Gate 2; edge-hardness encodes guarantee strength; Gate 2 spans mechanizable-to-non-mechanizable internally.
[O - Organization] Left-to-right; gates as rectangles in series; reject arrows drop down; accept continues right; Gate 2 visibly subdivided solid→dashed.
[P - Presentation] Flat vector, Okabe-Ito: light gray (raw input), blue #0072B2 (Gate 1), orange #E69F00 (Gate 2), bluish green #009E73 (pass/accept arrows + terminal), vermillion #D55E00 (reject arrows), black #000000 (connectors). 1pt strokes, no text in image.
[E - Exclusions] No "structural/semantic/Gate" words, no percentages, no field names, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
gate names, "structural/semantic" words, percentages, JSON text, field names, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Constrained Decoding — Token Masking at Generation Time
Priority: Critical
Trigger: MC + VG — a ≥3-step mechanism (candidate tokens → grammar/FSM mask → only valid tokens sampleable) that enforces validity rather than checking it; a mechanism not depictable from prose
Figure type: Mechanism cross-section
Concept statement: Constrained decoding enforces schema validity at generation time by masking, at each step, every token that would violate the grammar — so an invalid token is literally impossible to sample, making structure a guarantee rather than a hope.
Reader prior knowledge: Reader knows post-hoc parsing checks after the fact; has not seen the decode-time mask that zeroes out invalid logits before sampling.
Source anchor: Section: 6.2 Gate 1: structure is a solved, deterministic problem (Willard & Louf / Outlines)

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector mechanism cross-section, single column, white background, read left to right as one decoding step. On the left, draw a row of candidate token slots (six or seven small squares) in light gray — the full vocabulary at this step. In the middle, draw a grammar/finite-state-machine gate as a vertical masking bar in blue #0072B2 with small node-and-edge state glyphs inside it. Show the mask acting on the token row: a subset of token squares are blocked (overlaid with a solid vermillion #D55E00 block, zeroed) and a smaller subset passes through unblocked in bluish green #009E73 (grammar-valid continuations). On the right, a single sampling arrow in black #000000 draws only from the green-passed tokens into an output position. Keep exactly: token row, FSM mask bar, blocked subset, passed subset, sampling arrow, output slot — at most six component groups. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] A candidate-token row; an FSM/grammar mask; a blocked (zeroed) subset; a valid (passed) subset; a sampling arrow into one output slot. Preserve: the mask acts before sampling; blocked tokens are impossible to sample; only grammar-valid tokens remain.
[O - Organization] Left-to-right single decoding step: vocabulary row → mask bar → split into blocked vs passed → sample into output.
[P - Presentation] Flat vector, Okabe-Ito: light gray (candidate tokens), blue #0072B2 (FSM/grammar mask), vermillion #D55E00 (blocked tokens), bluish green #009E73 (valid passed tokens), black #000000 (sampling arrow). 1pt strokes, no text in image.
[E - Exclusions] No token text, no logit numbers, no grammar notation, no human figures, no shadows, no gradients, no red-green adjacency beyond the deliberate blocked/passed contrast (keep them spatially separated, not interleaved).

### Block 3 — Negative Prompt
token characters, logit numbers, grammar notation, regex text, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: Semantic Validity Is a Spectrum of Mechanizability
Priority: Important
Trigger: VG — a hierarchy/ordering claim: five semantic check types ranked from fully mechanizable to non-mechanizable; structural, not depictable from text
Figure type: Hierarchy/taxonomy
Concept statement: Semantic validity is not one thing but a spectrum — range checks, enum membership, referential integrity, and cross-field consistency are mechanizable, while factual-value correctness generally is not and pushes into factual-validation or human territory.
Reader prior knowledge: Reader knows the schema can't reach truth; has not seen the five check types ordered along a mechanizability gradient with a clear cutoff.
Source anchor: Section: 6.3 Gate 2: structure passed — are the values right?

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector ordered taxonomy, single column, white background, read top to bottom as decreasing mechanizability. Draw five stacked horizontal tiers as rectangles. Tiers one through four (range, enum, referential integrity, cross-field consistency) in bluish green #009E73, each with a solid edge — the mechanizable band. Tier five (factual-value correctness) in orange #E69F00 with a dashed edge — the non-mechanizable tier. Draw a single horizontal divider line in black #000000 between tier four and tier five marking the cutoff. On the left, a vertical gradient bar in light gray fading downward indicates mechanizability decreasing top to bottom. From tier five, draw one exit arrow in vermillion #D55E00 pointing sideways/away (hands off to factual validation / human). Keep exactly five tiers, one divider, one fade bar, one hand-off arrow — eight components. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Five stacked check-type tiers (four mechanizable, one not), a cutoff divider between tier four and five, a left-side mechanizability fade, and a hand-off arrow from the bottom tier. Preserve: the four-vs-one split, the top-to-bottom decreasing mechanizability, and that the bottom tier exits to another process.
[O - Organization] Vertical stack, top-to-bottom; solid-edged green tiers above a black divider, dashed orange tier below; fade bar on the left; hand-off arrow leaving the bottom tier.
[P - Presentation] Flat vector, Okabe-Ito: bluish green #009E73 (mechanizable tiers), orange #E69F00 (non-mechanizable tier), light gray (fade bar), vermillion #D55E00 (hand-off arrow), black #000000 (divider). 1pt strokes, no text in image.
[E - Exclusions] No check-type names, no code, no field examples, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
check-type names, "range/enum/referential" words, code snippets, field examples, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: Retry Loop Is Resampling, Not Enforcement
Priority: Important
Trigger: MC + VG — a process contrast: a retry loop (sample → fail → resample → hope) versus constrained decoding (guaranteed valid first sample); the distinction is structural and load-bearing
Figure type: Comparison panels
Concept statement: A retry loop draws another sample and hopes it parses — raising apparent success without guaranteeing anything — whereas constrained decoding makes the failure impossible on the first sample; resampling persistence does not belong in a reliability number.
Reader prior knowledge: Reader knows retries are common; has not seen retry (a loop that may exhaust and escalate) set against enforcement (no loop needed) to expose the false reliability claim.
Source anchor: Section: 6.4 The two-gate pipeline, and the retry trap

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector two-panel comparison, single column, white background, panels stacked. Top panel (retry / resampling): a model box in orange #E69F00 emits an output into a validator diamond; a fail arrow in vermillion #D55E00 loops back up into the model input (resample), forming a visible cycle; after a bounded number of loops draw a final exit arrow in vermillion #D55E00 to a small escalation node (exhausted). Mark the loop with a small counter-stack glyph in black #000000 to imply "up to N tries." Bottom panel (enforcement / constrained decoding): a model box in blue #0072B2 passes through an inline grammar-mask gate in bluish green #009E73 straight to a guaranteed-valid output terminal in bluish green #009E73 — no loop, one pass. Keep the two panels parallel so the contrast is loop-present versus loop-absent. Exactly: two model boxes, one validator diamond + loop, one mask gate, two terminals — at most seven components. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Top: model → validator → fail loop back to model (bounded), then escalation exit. Bottom: model → inline mask → guaranteed-valid terminal, no loop. Preserve: retry is a cycle that may exhaust; enforcement is a single guaranteed pass; the structural difference is presence vs absence of the resampling loop.
[O - Organization] Two stacked parallel panels; top contains a back-loop and an escalation exit; bottom is strictly left-to-right with no loop.
[P - Presentation] Flat vector, Okabe-Ito: orange #E69F00 (retry model), blue #0072B2 (enforced model), bluish green #009E73 (mask gate + guaranteed terminal), vermillion #D55E00 (fail/loop + escalation arrows), black #000000 (counter glyph + forward arrows). 1pt strokes, no text in image.
[E - Exclusions] No "retry/enforcement/N tries" words, no percentages, no code, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
"retry/enforcement" words, "N tries" text, percentages, code snippets, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — two-gate pipeline: STATIC SUFFICIENT — a two-stage gated flow reads fully in one frame and serves as a reference diagram.
FIGURE 2 — constrained decoding mask: VIDEO CANDIDATE — token masking is inherently step-wise; animating one decode step (full vocabulary → mask drops invalid tokens → sample from survivors) makes "invalid is impossible" concrete in a way a static frame only implies.
FIGURE 3 — mechanizability spectrum: STATIC SUFFICIENT — an ordered taxonomy is a static reference scanned at a glance.
FIGURE 4 — retry vs enforcement: VIDEO CANDIDATE — the retry loop is temporal by nature; animating it spinning through samples (and sometimes exhausting) beside the one-pass enforcement path would dramatize "resampling, not enforcement."
Recommendation: Build all four as static; Figures 2 and 4 are the strongest video upgrades since both hinge on a temporal mechanism (per-step masking; the resampling cycle), while Figures 1 and 3 are best left as static reference diagrams.
