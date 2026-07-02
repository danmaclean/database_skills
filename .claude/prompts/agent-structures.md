# Agent prompt — convert the structural-databases tutorial

You are an autonomous conversion agent working in `/Users/macleand/Desktop/database_skills`.

## Your task
Convert the source `learnr` tutorial into chapter 3 of this Quarto book.

- **Source (read-only):** `../structural_dbs/structural_databases.Rmd`
- **Target:** `03-structures.qmd` (currently a stub — replace the whole file)

## Read first (in this order)
1. `CLAUDE.md` — what this project is and the R-free rule.
2. `.claude/SPEC.md` — **the contract.** Follow the `question()` → naquiz transform, the "Why?"
   callout pattern, prose rules, and the flag-don't-fix policy exactly.
3. `.claude/MEMORY.md` — naquiz syntax + the CSS-fix warning.

## Do
- Port all prose verbatim, lightly polished, in the author's voice. Keep the sample FASTA/protein
  sequence blocks, every URL (PDBe, Phyre2, Robetta, TM-align, the `danmaclean.github.io`/
  `raw.githubusercontent.com` links), the `.pdb`/accession IDs, and command-line blocks exactly.
  Keep the `## Aims` opener and the `## Wrap up` closer.
- Convert **every** `question()` to a naquiz `.question` block, mark the single correct choice
  `.correct-choice`, and add a `## Why?` `callout-note` after each (1–3 sentences, grounded in how
  PDBe sequence search / homology modelling / TM-score work — written fresh).
- Stay **R-free**: no `{r}`/`{webr}`/`{ojs}`, no data, no images.

## Chapter-specific flags (confirm, handle as SPEC says, and record in `FLAGS-structures.md`)
- **`## Aims` copy-paste leak:** the source says "we carry out expression estimates for the data we
  downloaded" — lifted from the transcriptome tutorial and wrong for a structures tutorial. This one
  is a strong candidate to correct (propose: something like "…in which we search for a known structure
  and predict the structure of a protein of our own"). Propose the fix in FLAGS; keep the source
  wording only if you're unsure — but flag it either way.
- Some questions have only two options (`tm-align-2`, `tm-align-3` has four) — that's fine; preserve.
- Watch for stale tool URLs (e.g. the old `zhanglab.ccmb.med.umich.edu` TM-align host) and record
  them as a longevity caveat — don't change URLs on your own.

## Deliverables
1. `03-structures.qmd` (replaces the stub).
2. `FLAGS-structures.md` in the repo root: `## Content quirks flagged` + `## New explanations to review`.

## Verify before you finish
- `quarto render 03-structures.qmd` → exit 0, no error.
- `grep -nE '\{(r|webr|ojs)' 03-structures.qmd` → no matches.
- Every `.question` has exactly one `.correct-choice` and a following `## Why?` callout.

## Don't
- Don't touch the other chapters, `_quarto.yml`, or the naquiz extension.
- Don't add R/webR/renv. Don't commit `docs/`. Don't merge to `master`.
- Don't "correct" the science on your own authority — flag it (the Aims leak included: propose, don't impose).