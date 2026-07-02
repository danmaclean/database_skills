# CLAUDE.md

Guidance for Claude Code (and any agent/session) working in this repository.

## ▶ START HERE (read at the start of every session)

This repo carries its own working context in **`.claude/`** (committed, so it travels with the branch). Read these first:

- **`.claude/NEXT-SESSION.md`** — current status, what's in flight, the exact next steps. *Read first.*
- **`.claude/SPEC.md`** — the conversion specification: exactly how to turn a source `learnr` tutorial into a chapter of this book. **This is the contract downstream agents follow.**
- **`.claude/ROADMAP.md`** — the plan, the phases, the decisions behind them.
- **`.claude/MEMORY.md`** — durable gotchas and conventions (naquiz CSS fix, flag-don't-fix policy, etc.).
- **`.claude/prompts/`** — ready-to-run prompts for the downstream conversion + integration agents.

## What this is

Source for the Quarto book **"Finding and Using Biological Databases"** by Dan MacLean — three self-checking tutorials that school and practice learners on public **transcriptome**, **genome**, and **structural** databases. It is part of an MSc bioinformatics course, allied to the "Intro to Stats" handbook (`../intro_to_stats`).

**Origin:** these were three separate `learnr`/Shiny apps on shinyapps.io
(`TeamMacLean/transcriptome_dbs`, `genome_dbs`, `structural_dbs`). We are consolidating them into
**one static Quarto book that runs entirely in the browser**, removing the Shiny server dependency
and its hosting cost. The interactive multiple-choice self-checks are provided by the
[`naquiz`](https://github.com/nareal/naquiz) Quarto filter.

Published site (once live): https://danmaclean.github.io/database_skills/

## The single most important fact

**This book contains no R.** The source tutorials use `learnr::question()` for multiple-choice
questions and nothing else — there are **no code exercises, no computation, no data, no images**.
So, unlike the stats book, this project needs **no `renv`, no R, no webR/quarto-live, no `itssl`**.
It is pure Quarto markdown + the `naquiz` Lua filter. **Keep it R-free** (see `.claude/SPEC.md`).

## Development process

- **Never commit directly to `master`.** Work on a feature branch, merge via a reviewed PR. `master`
  stays known-good and renderable.
- **`master` is the live source.** `render.yml` gates PRs/feature pushes on a clean `quarto render`;
  `publish.yml` renders on pushes to `master` and publishes to the **`gh-pages`** branch, which
  GitHub Pages serves. **Never commit `docs/`** (git-ignored build output).
- **Render before merging.** A chapter isn't done until `quarto render` completes with no error.

## Build & preview

Pure Quarto — you only need Quarto installed (no R, no `renv`).

```sh
quarto render                       # build the whole book into docs/
quarto preview                      # live-reload preview while editing
quarto render 01-transcriptomes.qmd # render a single chapter
```

## Conventions

- **naquiz extension is vendored** at `_extensions/nareal/naquiz` and carries a **CSS scope fix**
  (`.choices`-scoped selectors) — re-running `quarto add nareal/naquiz` would overwrite it. See
  `.claude/MEMORY.md`.
- **Migrate content verbatim; flag quirks, don't silently "fix" them.** The source tutorials have
  known content bugs (listed in `SPEC.md`). Surface them for the author; don't quietly rewrite.
- **Chapter structure & voice** — see `.claude/SPEC.md`.
