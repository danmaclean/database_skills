# SPEC — converting a source tutorial into a chapter of this book

This is the contract every conversion agent follows. Read it fully before editing a chapter.

## Goal

Turn each source `learnr`/Shiny tutorial into one chapter of a single static Quarto book that runs
**entirely in the browser** — no Shiny server, no R, no hosting cost. Interactive multiple-choice
self-checks are provided by the **`naquiz`** Quarto filter (already vendored at
`_extensions/nareal/naquiz`). Enrich each question with a short **"Why?"** explanation.

## Source → target map

| Source repo (sibling dir)            | Source file                      | Target chapter          |
|--------------------------------------|----------------------------------|-------------------------|
| `../transcriptome_dbs`               | `transcriptomes.Rmd`             | `01-transcriptomes.qmd` |
| `../genome_dbs`                      | `genomes.Rmd`                    | `02-genomes.qmd`        |
| `../structural_dbs`                  | `structural_databases.Rmd`       | `03-structures.qmd`     |

The three source repos are **read-only** for this project — read from them, write only into
`database_skills`. Each target chapter currently holds a stub; replace the whole stub file.

## Non-negotiable principles

1. **Stay R-free.** These tutorials contain no code exercises and no computation. Do **not** add
   `{r}` chunks, `knitr`, `renv`, or webR. The one exception in the source — a single inline
   `` `r round((531/591)*100, 2)` `` in the genome tutorial — must be replaced with the literal
   value **`89.85`**. After conversion, the book must render with Quarto's markdown path alone.
2. **Migrate prose verbatim, then lightly polish.** Preserve the author's wording, the biology, the
   command-line examples, every URL, and every accession/ID exactly. You may fix obvious typos and
   smooth a sentence, but do not rewrite the tutorial or change what it teaches.
3. **Flag content quirks; never silently "fix" them.** The source has known bugs (see
   "Known flags" below, and watch for more). For each, keep the source behaviour, add an HTML
   comment at the point of the quirk, and record it in the chapter's `FLAGS-<chapter>.md` report for
   the author to decide on. This is a firm author policy: *surface, don't overrule.*
4. **Voice.** The author is a knowledgeable peer-mentor to biologists (see `../intro_to_stats/CLAUDE.md`
   for the full style). These database tutorials are more matter-of-fact than the stats book — keep
   their practical, walk-you-through tone; don't force in jokes. Contractions and direct "you"/"we"
   are welcome. Any **new** prose you write (the "Why?" notes, an opener line) must sound like him.

## The core transform: `question()` → naquiz block

Every `learnr::question()` becomes a naquiz `.question` fenced-div block, followed by a collapsible
**"Why?"** callout. Use this exact structure (fence colon-counts matter — outer `.question` uses 5
colons, `.choices` 4, each `.choice` 3):

**Source (`learnr`):**
```r
```{r cc-taxid}
question(
  "What is the species taxon ID for _C.concisus_?",
  answer("11485"), answer("199", correct = TRUE), answer("33237")
)
```
```

**Target (naquiz + Why?):**
```markdown
:::::{.question}
What is the species taxon ID for *C. concisus*?

::::{.choices}
:::{.choice}
11485
:::

:::{.choice .correct-choice}
199
:::

:::{.choice}
33237
:::
::::
:::::

:::{.callout-note collapse="true"}
## Why?
Every taxon has one stable NCBI Taxonomy ID; for *Campylobacter concisus* it's **199**. The other
numbers are IDs for different taxa — always confirm the ID before trusting a database search.
:::
```

Rules for the transform:

- **Preserve answer order and wording exactly**, including the distractors. Convert `_italic_`
  markup and species names to Quarto markdown (`*C. concisus*`).
- **Mark the correct answer** by adding `.correct-choice` to that choice's div. In the source the
  correct one is the `answer(..., correct = TRUE)`.
- **The chunk label** (e.g. `cc-taxid`) is dropped — naquiz needs no id. Don't turn it into a Quarto
  section id.
- **Strip stray characters** in option text: a literal tab (`\t`), a trailing comma in `answer("x",)`,
  numeric-not-string answers like `answer(38)` → render as plain `38`.
- **Multiple `correct = TRUE` in one question is a bug, not a feature.** naquiz questions have a
  single correct choice. Where the source marks more than one correct, mark the single *intended*
  answer `.correct-choice`, and **flag it** (see the transcriptome `dload-2` case below).

### The "Why?" callout (enrichment — required)

The source questions carry **no** feedback, so you are writing these fresh. Each `.question` gets a
`:::{.callout-note collapse="true"}` titled `## Why?` immediately after it. Keep it to 1–3 sentences:
say *why* the correct answer is right and, where useful, why a tempting distractor is wrong. Ground it
in the biology / how the database works — this is the real teaching value. Because this is new prose,
list every "Why?" you write in the FLAGS report under a "New explanations to review" heading so the
author can check the science.

## Chapter structure

- **Title:** an H1 matching the source `title:` (already set in each stub — keep it).
- **Keep the source's `## Aims` section** as the opener, lightly polished. Do not invent a
  Questions/Objectives/Keypoints block (that's the stats book's convention, not this one).
- Preserve the source's `##` section headings and order.
- **Command-line examples and file/output samples** stay as plain fenced code blocks (```` ``` ````),
  exactly as in the source. They are illustrative, not runnable — do not make them `{webr}` or `{bash}`.
- **Wrap-up:** keep the source's closing "Wrap up" section.

## Known flags (found during planning — confirm and record these, and add any others you find)

**`01-transcriptomes.qmd`:**
- `dload-2` ("How many reads were downloaded for 8h?") marks **two** answers `correct = TRUE`
  (`10,691,872` and `9,339,062`). `9,339,062` is the 14h count (per `dload-1`), so the intended 8h
  answer is `10,691,872`. Mark that one correct; flag the double-correct.
- `taxid-1` ("What *strain* of E. coli was used?") marks `"K12f"` correct — looks like a typo for
  `"K12"`. Keep the source string; flag as a probable typo.
- `geoacc-3` has an option with a literal tab before `SRP069289` — strip the tab; cosmetic.

**`02-genomes.qmd`:**
- Inline `` `r round((531/591)*100, 2)` `` in the BUSCO wrap-up → replace with literal **`89.85`**
  (keeps the book R-free). Not a content flag, just the R-free rule.

**`03-structures.qmd`:**
- The `## Aims` section says "we carry out expression estimates for the data we downloaded" — a
  copy-paste leak from the transcriptome tutorial; wrong for a structures tutorial. Propose a
  corrected sentence in the FLAGS report but keep/settle per author; this one is a strong candidate
  to fix given it's plainly wrong. Flag either way.

**All chapters — maintenance caveat (record once, don't fix):** several answers are *live database
facts that drift over time* (e.g. "about 1,200 E. coli genomes", "about 250 assemblies"). Taxon IDs
and accessions are stable; result counts are not. Note this in the FLAGS report as a longevity risk.

## Deliverables per conversion agent

1. The converted chapter `.qmd`, replacing the stub.
2. A **`FLAGS-<chapter>.md`** file in the repo root (e.g. `FLAGS-transcriptomes.md`) with:
   `## Content quirks flagged` (each: location, what's wrong, what you did), and
   `## New explanations to review` (each "Why?" you wrote, for science-check).
3. The chapter must **render clean on its own**: `quarto render 0X-<chapter>.qmd` exits 0.

## Verification (every agent, before declaring done)

- `quarto render 0X-<chapter>.qmd` → exit 0, no error.
- Grep your chapter for accidental `{r`/`{webr`/`{ojs` — there must be none.
- Confirm every `.question` has exactly one `.correct-choice` and a following `## Why?` callout.
- The integration agent additionally runs a **full `quarto render`** and a rendered-DOM check that no
  naquiz CSS bleed occurred (see `.claude/MEMORY.md` — the `.choices`-scoped fix must be intact).

## What NOT to do

- Don't add R, webR, `renv`, or data/image assets.
- Don't commit `docs/` (git-ignored).
- Don't overwrite `_extensions/nareal/naquiz` via `quarto add` (loses the CSS fix).
- Don't merge to `master` directly — branch + PR.
- Don't rewrite the tutorial's teaching or "correct" the science on your own authority — flag it.
