# Agent prompt — convert the transcriptomes tutorial

You are an autonomous conversion agent working in `/Users/macleand/Desktop/database_skills`.

## Your task
Convert the source `learnr` tutorial into chapter 1 of this Quarto book.

- **Source (read-only):** `../transcriptome_dbs/transcriptomes.Rmd`
- **Target:** `01-transcriptomes.qmd` (currently a stub — replace the whole file)

## Read first (in this order)
1. `CLAUDE.md` — what this project is and the R-free rule.
2. `.claude/SPEC.md` — **the contract.** The full `question()` → naquiz transform, the "Why?" callout
   pattern, prose rules, and the flag-don't-fix policy. Follow it exactly.
3. `.claude/MEMORY.md` — naquiz syntax + the CSS-fix warning.

## Do
- Port all prose verbatim, lightly polished, in the author's voice. Keep every URL, accession, run/
  sample ID, and command-line block exactly. Keep the `## Aims` opener and the `## Wrap up` closer.
- Convert **every** `question()` to a naquiz `.question` block (5/4/3 colon nesting), marking the
  single correct choice `.correct-choice`, and add a collapsible `## Why?` `callout-note` after each
  (1–3 sentences, grounded in how GEO/SRA/Ensembl work — you are writing these fresh).
- Stay **R-free**: no `{r}`/`{webr}`/`{ojs}`, no data, no images.

## Chapter-specific flags (confirm, handle as SPEC says, and record in `FLAGS-transcriptomes.md`)
- **`dload-2`** ("How many reads were downloaded for 8h?") marks **two** answers correct
  (`10,691,872` and `9,339,062`). `9,339,062` is the 14h count. Mark **`10,691,872`** correct; flag
  the double-correct for the author.
- **`taxid-1`** ("What *strain* of E. coli?") marks `"K12f"` correct — probable typo for `"K12"`.
  Keep the source string as the correct choice; flag as a likely typo.
- **`geoacc-3`** has an option with a literal tab before `SRP069289` — strip the tab (cosmetic).
- Watch for others and record them too.

## Deliverables
1. `01-transcriptomes.qmd` (replaces the stub).
2. `FLAGS-transcriptomes.md` in the repo root: `## Content quirks flagged` + `## New explanations to
   review` (list every "Why?" you wrote, for the author's science-check).

## Verify before you finish
- `quarto render 01-transcriptomes.qmd` → exit 0, no error.
- `grep -nE '\{(r|webr|ojs)' 01-transcriptomes.qmd` → no matches.
- Every `.question` has exactly one `.correct-choice` and a following `## Why?` callout.

## Don't
- Don't touch the other chapters, `_quarto.yml`, or the naquiz extension.
- Don't add R/webR/renv. Don't commit `docs/`. Don't merge to `master`.
- Don't "correct" the science on your own authority — flag it.