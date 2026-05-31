# CAJAL SVG Generation Log

## Run: 2026-05-30

Book: Validating Output from AI Systems (`validating-output-from-ai-systems`)
Style guide: embedded Brutalist D3 SVG Style Guide (the referenced `NEU/` and `brutalist/` design-guide paths do not exist in this book; the full Style Guide in the run prompt was used and matched against existing house SVGs).
Environment fix: the book's `node_modules` was a symlink to a dead session path, so `sharp`/`glob` could not resolve. Replaced the broken symlink with a fresh `npm install` (sharp ^0.33, glob ^10) before PNG conversion.

### 00-frontmatter-cajal.md
- figures found: 0
- SVGs generated: 0
- skipped (existing): 0

No figure-worthy zones (title/copyright/dedication/preface). Zero figures is the correct CAJAL output.

### 00-introduction-cajal.md
- figures found: 1
- SVGs generated: 1
- skipped (existing): 0

| File | Figure title | Type | Status |
|---|---|---|---|
| 00-introduction-fig-01.svg | The execution–judgment boundary | Comparison panels | generated |

### 99-back-matter-cajal.md
- figures found: 0
- SVGs generated: 0
- skipped (existing): 0

No figure-worthy zones (acknowledgments/bio/refs/glossary). Zero figures is the correct CAJAL output.

### 01–13 content chapters (cajal.md files)
- figures found: 56 (across 13 files)
- SVGs generated: 0
- skipped (existing): 56

All 56 figures already had SVGs in `images/` (generated 2026-05-29) and were skipped per the collision rule. The Ch.10 and Ch.11 prose edits made on 2026-05-30 were citation-only fixes ("Lucy"→"Lisanne" Bainbridge; Buçinca→de Jong et al.) and touch no figure; their cajal.md files reference none of those citations, so no re-scan or regeneration was warranted.

| File | Figures | Status |
|---|---|---|
| 01-why-validating-ai-output-is-different-cajal.md | 4 | all skipped (existing) |
| 02-the-deterministic-floor-cajal.md | 4 | all skipped (existing) |
| 03-validating-generated-code-cajal.md | 5 | all skipped (existing) |
| 04-validating-factual-claims-cajal.md | 4 | all skipped (existing) |
| 05-validating-reasoning-chains-cajal.md | 4 | all skipped (existing) |
| 06-validating-structured-output-cajal.md | 4 | all skipped (existing) |
| 07-validating-agentic-task-execution-cajal.md | 5 | all skipped (existing) |
| 08-llm-as-judge-uses-and-failure-modes-cajal.md | 5 | all skipped (existing) |
| 09-process-reward-models-cajal.md | 5 | all skipped (existing) |
| 10-human-validation-and-automation-bias-cajal.md | 4 | all skipped (existing) |
| 11-building-a-validation-pipeline-cajal.md | 4 | all skipped (existing) |
| 12-scalable-oversight-cajal.md | 4 | all skipped (existing) |
| 13-capstone-designing-a-validation-strategy-cajal.md | 4 | all skipped (existing) |

---

## Summary
Total cajal.md files processed: 16 (3 new this run + 13 pre-existing)
Total figures parsed: 57
Total SVGs generated this run: 1
Total skipped (already exist): 56
PNG conversion: run completed (1 converted, 56 up-to-date). All 57 SVGs now have matching 300 DPI PNGs.

Chapter files were not modified. No markdown image references or Prompts-section entries were inserted — those belong to the enrichment pass.
