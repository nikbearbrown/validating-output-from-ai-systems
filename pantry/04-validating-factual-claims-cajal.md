# CAJAL Figure Intelligence — Chapter 4 — Validating Factual Claims

Source: chapters/04-validating-factual-claims.md
Mode: /scan silent
Date: 2026-05-29

## Density Recommendation
4 figure candidates. Mechanistic density. The chapter turns on multi-stage causal mechanisms (two-gate checking, RAG relocation, layered pipeline) and one quantitative claim set that all reward figural treatment over prose.

## Figure 1: Three Properties of a Citation — Existence, Support, Correctness
Priority: Critical
Trigger: VG — structural claim about three independent axes and which are mechanical vs. judgmental, not depictable from running text
Figure type: Conceptual map
Concept statement: A citation has three independent, separately-checkable properties — it exists (mechanical), it supports the claim (judgment), it is correct (domain knowledge) — and conflating them is the source of most factual-validation error.
Reader prior knowledge: Reader knows the Mata v. Avianca fabrication story from §4.1; has not seen the three axes laid out as independent dimensions where a citation can pass one and fail another.
Source anchor: Section: 4.2 Three properties of a citation: exists, supports, correct

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector conceptual map, single column, on a white background, showing three stacked horizontal gate-bands representing three independent properties of a citation, read top to bottom as increasing difficulty. Draw band one as a solid-edged rectangle (mechanical / existence) in blue #0072B2; draw band two as a dashed-edged rectangle (judgment / support) in orange #E69F00; draw band three as a dotted-edged rectangle (domain knowledge / correctness) in light gray. To the left, place a single vertical entry arrow in black #000000 carrying a small citation token (a document glyph) that passes downward through all three bands in sequence. To the right of each band, show a small branching pass/fail fork: a green #009E73 short arrow continuing down and a vermillion #D55E00 short arrow exiting sideways. Keep exactly three bands, one entry token, three forks — at most seven labeled components. Use 1pt strokes, flat fills, no text, no shadows, no gradients. Convey that the bands are independent and sequential, that edge-style encodes mechanizability, and that a token can pass one band and fail another.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Three independent property-bands (existence, support, correctness); one entering citation token; three pass/fail forks. Preserve: the bands are independent (passing one says nothing about the others) and ordered by increasing non-mechanizability.
[O - Organization] Three horizontal bands stacked vertically top-to-bottom; single vertical entry arrow on the left threading through all three; a two-pronged pass(down)/fail(sideways) fork on the right of each band.
[P - Presentation] Flat vector, Okabe-Ito: blue #0072B2 (mechanical band), orange #E69F00 (judgment band), light gray (correctness band), green #009E73 (pass arrows), vermillion #D55E00 (fail arrows), black #000000 (entry arrow/token). Edge style encodes mechanizability: solid / dashed / dotted. 1pt strokes, no text in image.
[E - Exclusions] No text labels, no axis titles, no numeric values, no human figures, no document realism beyond a simple glyph, no shadows, no gradients, no red-green pairing on adjacent elements.

### Block 3 — Negative Prompt
property names, axis labels, "exists/supports/correct" words, percentages, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 2: Two-Gate Citation Check — Existence Gate then Support Gate
Priority: Critical
Trigger: MC — a ≥3-step interdependent process (decompose → existence gate → support gate) where one gate is deterministic and one judgmental, with distinct reject paths
Figure type: Process flowchart
Concept statement: Factual validation runs as two sequential gates — a deterministic citation-existence check that rejects fabrications, then a judgmental claim-support check that catches the real-citation-wrong-claim class the first gate waves through.
Reader prior knowledge: Reader has the three-axis distinction from §4.2 and the span-decomposition idea from §4.3; has not seen the two checks composed as an ordered gating flow with separate failure exits.
Source anchor: Section: 4.5 The layered method, and where the human must own the residual (steps 1, 2, 4)

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector process flowchart, single column, white background, flowing top to bottom. Start with one input node (an answer block) in light gray at top. An arrow leads to a decompose step (a node that fans into several small claim tokens) in blue #56B4E9. From the claim tokens a single arrow enters Gate 1, drawn as a solid-edged diamond (deterministic existence check) in blue #0072B2, with a vermillion #D55E00 "reject — fabricated" arrow exiting left and a green #009E73 "pass" arrow continuing down. The pass arrow enters Gate 2, drawn as a dashed-edged diamond (judgmental support check) in orange #E69F00, with a vermillion #D55E00 "reject — unsupported" arrow exiting left and a green #009E73 "accept" arrow exiting to a final terminal node at the bottom. Use exactly: input, decompose, Gate 1 diamond, Gate 2 diamond, two reject arrows, one terminal — seven components. Encode determinism with solid edges and judgment with dashed edges. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Input answer; decompose-into-claims step; Gate 1 (existence, deterministic, solid diamond); Gate 2 (support, judgmental, dashed diamond); two reject exits; one accept terminal. Preserve: Gate 1 precedes Gate 2; each gate has its own distinct reject path; the gates differ in mechanizability (solid vs dashed edge).
[O - Organization] Vertical top-to-bottom flow; gates as diamonds in series; reject arrows exit left, pass/accept arrows continue down; decompose node fans the input into several small claim tokens before Gate 1.
[P - Presentation] Flat vector, Okabe-Ito: light gray (input), blue #56B4E9 (decompose/claims), blue #0072B2 (Gate 1 deterministic), orange #E69F00 (Gate 2 judgmental), green #009E73 (pass/accept arrows), vermillion #D55E00 (reject arrows), black #000000 (connectors). 1pt strokes, no text in image.
[E - Exclusions] No gate names, no step labels, no "exists/supports" words, no percentages, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
gate labels, step names, "existence/support" words, numbers, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 3: RAG Relocates the Failure — From Generator to Retriever
Priority: Critical
Trigger: VG + MC — a systems claim about where the failure mode physically moves (generator → retriever) plus the multi-component retrieve-then-generate pipeline; not depictable from text
Figure type: Systems diagram
Concept statement: RAG does not remove factual failure; it relocates it upstream — before RAG the generator confabulates, after RAG the answer is only as correct as retrieval, so a confident, well-cited answer can still be wrong because retrieval was wrong.
Reader prior knowledge: Reader knows RAG conditions generation on retrieved documents; has not seen the failure "arrow of blame" move from the generator to the retriever depicted as a before/after relocation.
Source anchor: Section: 4.4 RAG reduces hallucination — and relocates the failure into retrieval

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector systems diagram, single column, white background, as two stacked horizontal panels sharing the same left-to-right flow. Top panel (pre-RAG): a query node in light gray flows by black #000000 arrow directly into a generator box in blue #0072B2, which emits an answer node; mark a failure burst (a small starburst glyph) on the generator in vermillion #D55E00 to show confabulation originates there. Bottom panel (post-RAG): the same query node flows first into a retriever box in orange #E69F00, which passes retrieved-document tokens by arrow into the generator box in blue #0072B2, which emits the answer; move the vermillion #D55E00 failure starburst onto the retriever box, and draw a faint dashed black arrow from retriever to answer indicating the error propagates downstream. Keep exactly: query, generator, answer (top); query, retriever, generator, answer (bottom) with one relocated failure burst — at most eight components. 1pt strokes, flat, no text, no shadows, no gradients.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Two panels: pre-RAG (query → generator → answer, failure on generator) and post-RAG (query → retriever → generator → answer, failure on retriever). Preserve: the failure starburst is the same object relocated from generator (top) to retriever (bottom); retrieval feeds generation.
[O - Organization] Two horizontal panels stacked vertically, identical left-to-right flow axis, so the reader's eye reads the relocation by comparing the same position across panels. Retriever inserted upstream of generator in the lower panel.
[P - Presentation] Flat vector, Okabe-Ito: light gray (query/answer nodes), blue #0072B2 (generator), orange #E69F00 (retriever), vermillion #D55E00 (failure starburst), black #000000 (flow arrows; dashed for downstream propagation). 1pt strokes, no text in image.
[E - Exclusions] No "RAG/retriever/generator" words, no percentages, no document realism beyond simple tokens, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
component names, "RAG/retriever/generator" words, percentages, reduction figures, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Figure 4: Citation Corpus — Existence Rate and Unsupported Share
Priority: Supplementary
Trigger: PQ — percentages/magnitudes (existence ≤ ~0.475; 50–90% of responses not fully supported) that read as bars far better than as prose
Figure type: Statistical/quantitative
Concept statement: The corpus evidence is grim on both axes — across generated citations no model exceeded a ~0.475 existence rate, and among citations that do exist, 50–90% of responses are not fully supported.
Reader prior knowledge: Reader has the existence-vs-support distinction; has not seen the two magnitudes set side by side to show both gates fail at high rates.
Source anchor: Section: 4.2 (existence ≤ ~0.475; 50–90% unsupported) — flagged [verify] in chapter

### Block 1 — Illustrae Paste Block
Create a blank unannotated flat vector bar chart, single column, white background, y-axis starting at zero and running to 100 percent. Draw two grouped bars on a common zero baseline. Bar one represents the maximum citation existence rate at roughly 47.5 percent of full height, in blue #0072B2. Bar two is drawn as a range band rather than a single bar — a vermillion #D55E00 vertical band spanning roughly 50 to 90 percent of full height — representing the unsupported-response share among existing citations. Keep the y-axis as a plain vertical line with light gray gridlines at even intervals; no tick numbers, no labels. Exactly two quantitative marks (one solid bar, one range band) plus axis — at most four components. Use flat fills, 1pt strokes, no shadows, no gradients, no text.

### Block 2 — Full SCOPE Prompt
[S - Specification] Single-column 89mm textbook figure, 300 DPI, vector, white background, unannotated.
[C - Content] Two quantitative marks on one zero-based y-axis: a single bar near 47.5% (existence rate ceiling) and a range band spanning ~50–90% (unsupported share). Preserve: y-axis from zero; the second value is a range, not a point.
[O - Organization] Vertical bar chart, two positions along x; shared zero baseline; light gray horizontal gridlines; range value rendered as a vertical band.
[P - Presentation] Flat vector, Okabe-Ito: blue #0072B2 (existence bar), vermillion #D55E00 (unsupported range band), light gray (gridlines), black #000000 (axis line). 1pt strokes, no text in image, no tick numerals.
[E - Exclusions] No numeric labels, no axis titles, no legend text, no data callouts, no human figures, no shadows, no gradients, no red-green adjacency.

### Block 3 — Negative Prompt
numeric labels, percentage text, axis titles, legend, data callouts, tick numbers, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video Candidate Pass
FIGURE 1 — three properties of a citation: STATIC SUFFICIENT — no temporal/causal unfolding — the three axes are simultaneous and independent, best grasped at a glance.
FIGURE 2 — two-gate check: STATIC SUFFICIENT — sequential but small — the flow is two gates with reject paths and reads fully in a single static frame.
FIGURE 3 — RAG relocates the failure: VIDEO CANDIDATE — depicts a relocation (the failure burst migrating from generator to retriever) — an animated before/after that slides the failure point upstream would make the central "arrow of blame moves left" mechanism land harder than a static two-panel.
FIGURE 4 — corpus magnitudes: STATIC SUFFICIENT — a two-bar comparison needs no motion.
Recommendation: Build all four as static; promote Figure 3 (RAG relocation) to a short animated explainer if video resources allow, since the relocation is the chapter's signature mechanism.
