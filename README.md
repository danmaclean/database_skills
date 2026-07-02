# Finding and Using Biological Databases

Source for the Quarto book **"Finding and Using Biological Databases"** by Dan MacLean — a set of guided, self-checking tutorials that school and practice learners on using public **transcriptome**, **genome**, and **structural** databases.

These tutorials were previously three separate `learnr`/Shiny apps hosted on shinyapps.io
(`TeamMacLean/transcriptome_dbs`, `genome_dbs`, `structural_dbs`). They have been consolidated
into one static Quarto book that runs **entirely in the browser** — no Shiny server, no hosting cost.
The multiple-choice self-checks use the [`naquiz`](https://github.com/nareal/naquiz) Quarto filter.

Published site: https://danmaclean.github.io/database_skills/

## Build & preview

This is a **pure Quarto** project — there is no R code in the book, so no `renv`, no R packages,
and no webR. You only need Quarto installed.

```sh
quarto render        # build the whole book into docs/
quarto preview       # live-reload preview while editing
quarto render 01-transcriptomes.qmd   # render a single chapter
```

## How it is built and published

- **`master`** is the live source. Branch new work off `master`, land it via a reviewed PR.
- `.github/workflows/render.yml` renders the book on every PR / feature-branch push (the merge gate).
- `.github/workflows/publish.yml` renders on pushes to `master` and publishes to the **`gh-pages`**
  branch, which GitHub Pages serves.
- **Never commit `docs/`** — it is git-ignored build output.

## Working context

Durable context for anyone (human or agent) picking this up lives in **`.claude/`**:
`SPEC.md` (the conversion spec), `ROADMAP.md`, `NEXT-SESSION.md`, `MEMORY.md`, and per-agent
prompts in `.claude/prompts/`. Start with `CLAUDE.md`.