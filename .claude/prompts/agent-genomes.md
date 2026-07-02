# Agent prompt — convert the genomes tutorial

You are an autonomous conversion agent working in `/Users/macleand/Desktop/database_skills`.

## Your task
Convert the source `learnr` tutorial into chapter 2 of this Quarto book.

- **Source (read-only):** `../genome_dbs/genomes.Rmd`
- **Target:** `02-genomes.qmd` (currently a stub — replace the whole file)

## Read first (in this order)
1. `CLAUDE.md` — what this project is and the R-free rule.
2. `.claude/SPEC.md` — **the contract.** Follow the `question()` → naquiz transform, the "Why?"
   callout pattern, prose rules, and the flag-don't-fix policy exactly.
3. `.claude/MEMORY.md` — naquiz syntax + the CSS-fix warning.

## Do
- Port all prose verbatim, lightly polished, in the author's voice. Keep the `## Glossary`,
  every URL, taxon ID, accession (`GCA_…`, `PRJNA…`, `SRR…`), and command-line block exactly. Keep
  the `## Aims` opener and the `## Wrap up` closer.
- Convert **every** `question()` to a naquiz `.question` block, mark the single correct choice
  `.correct-choice`, and add a `## Why?` `callout-note` after each (1–3 sentences, grounded in how
  NCBI Taxonomy / ENA / SRA / assembly / BUSCO work — written fresh).
- Stay **R-free**: no `{r}`/`{webr}`/`{ojs}`, no data, no images.

## Chapter-specific handling (record in `FLAGS-genomes.md`)
- **Inline R in the BUSCO wrap-up:** the source has `` `r round((531/591)*100, 2)` ``. Replace it
  with the literal **`89.85`** so the book stays R-free. Note this in the FLAGS report (rule, not bug).
- `annot-1` has a numeric answer `answer(38)` — render as plain `38`.
- Watch for other quirks (drifting result counts like "about 250 assemblies" — record the longevity
  caveat once) and record them.

## Deliverables
1. `02-genomes.qmd` (replaces the stub).
2. `FLAGS-genomes.md` in the repo root: `## Content quirks flagged` + `## New explanations to review`.

## Verify before you finish
- `quarto render 02-genomes.qmd` → exit 0, no error.
- `grep -nE '\{(r|webr|ojs)' 02-genomes.qmd` → **no matches** (confirms the inline R is gone).
- Every `.question` has exactly one `.correct-choice` and a following `## Why?` callout.

## Don't
- Don't touch the other chapters, `_quarto.yml`, or the naquiz extension.
- Don't add R/webR/renv. Don't commit `docs/`. Don't merge to `master`.
- Don't "correct" the science on your own authority — flag it.