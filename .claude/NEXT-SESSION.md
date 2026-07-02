# NEXT SESSION — current status & what to do next

Live handoff. Update as work progresses. (Plan = `ROADMAP.md`; contract = `SPEC.md`; gotchas = `MEMORY.md`.)
Last updated: 2026-07-02.

## Where we are

**🎉 LIVE (2026-07-02).** All phases complete. The book is published at
**https://danmaclean.github.io/database_skills/** (HTTP 200, chapters serving).

- **`master`** = the trunk AND live source (scaffold + 3 converted chapters + author-approved fixes +
  FLAGS/AUTHOR-REVIEW docs), pushed to `github.com/danmaclean/database_skills` (public).
- **`gh-pages`** = CI-managed live Pages source (don't hand-edit). GitHub Pages source was
  **auto-enabled** by `quarto publish` (source = `gh-pages`, path `/`) — the manual Settings→Pages
  flip was NOT needed this time.
- **CI:** `publish.yml` renders + publishes to `gh-pages` on every push to `master` (green);
  `render.yml` gates PRs/feature branches. Both Quarto-only (no R/renv).
- **Author sign-off applied:** `taxid-1` K12f→K12, structures `dda321`→`d3d32a1`; `dload-2`
  single-correct and the structures Aims rewrite confirmed OK. Decision trail in `AUTHOR-REVIEW.md`.
- **Question tallies:** 12 + 14 + 11 = 37, each one `.correct-choice` + a "Why?" callout.

**▶ Remaining follow-ups (non-blocking):** retire the three shinyapps.io deployments now the new site
is confirmed live; optional in-browser click-test of naquiz on the live pages; future maintenance of
the drift-prone answer counts flagged in `AUTHOR-REVIEW.md`.

> **Incident/lesson (2026-07-02):** a stray no-op `fork` launched during scaffolding inherited the
> full plan and autonomously ran the whole conversion *in parallel* with the intended agents, doing
> git surgery (branch moves, an amend) that tangled history and briefly looked like data loss.
> Nothing was lost; history was rebuilt cleanly off `master`. **Lesson: never leave a `fork` running
> with a vague/no-op prompt — it will act on your whole context. Kill stray agents immediately.**

## Scaffold contents (Phase 0)

The seed repo holds:

- `_quarto.yml` — flat 3-chapter book, `naquiz` filter, `format: html` (cosmo), R-free.
- `_extensions/nareal/naquiz` — vendored, **with the `.choices`-scoped CSS fix already applied**.
- `index.qmd` — preface; `01-transcriptomes.qmd` / `02-genomes.qmd` / `03-structures.qmd` — **stubs**.
- `.github/workflows/render.yml` + `publish.yml` — Quarto-only CI (no R/renv).
- `CLAUDE.md` + `.claude/` (SPEC, ROADMAP, MEMORY, this file, and `prompts/`).
- **The skeleton renders clean** (`quarto render` → exit 0). Build output (`docs/`) is git-ignored.

Nothing is committed yet — the working tree is the initial state. (First commit + branch discipline
begins with Phase 1, or commit the scaffold as-is first; author's call.)

## ▶ NEXT — Phase 1: convert the three chapters

Run the three conversion agents (parallel — chapters are independent). Each has a ready prompt:

1. `.claude/prompts/agent-transcriptomes.md` → `01-transcriptomes.qmd` (from `../transcriptome_dbs/transcriptomes.Rmd`)
2. `.claude/prompts/agent-genomes.md` → `02-genomes.qmd` (from `../genome_dbs/genomes.Rmd`)
3. `.claude/prompts/agent-structures.md` → `03-structures.qmd` (from `../structural_dbs/structural_databases.Rmd`)

Each follows `SPEC.md`: prose verbatim + polish, every `question()` → naquiz `.question` block +
a "Why?" callout, stay R-free, and emit a `FLAGS-<chapter>.md` report.

Then **Phase 2 — integration/QA**: `.claude/prompts/agent-integration.md` (full render, CSS-bleed
check, consolidate FLAGS).

Then **Phase 3 — author review & go-live** (see ROADMAP): review FLAGS, create
`danmaclean/database_skills` on GitHub, push, build `gh-pages`, flip Pages source (manual UI step).

## Key facts a new session needs

- **No R anywhere.** Pure Quarto + naquiz. Do not add webR/renv/knitr. (One inline R expr in the
  genome source → replace with literal `89.85`.)
- **Migrate verbatim, flag don't fix.** Known content bugs are listed in `SPEC.md` ("Known flags").
- **Don't re-`quarto add` naquiz** — it would clobber the vendored CSS fix (see `MEMORY.md`).
- **Source repos are read-only** (`TeamMacLean/*` on `main`). Write only into `database_skills`.
- **Repo not yet on GitHub.** Creating `danmaclean/database_skills` + flipping Pages source are
  Phase-3 manual steps (the working PAT lacks Pages-admin; see `MEMORY.md`).