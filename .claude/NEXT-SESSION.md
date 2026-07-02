# NEXT SESSION — current status & what to do next

Live handoff. Update as work progresses. (Plan = `ROADMAP.md`; contract = `SPEC.md`; gotchas = `MEMORY.md`.)
Last updated: 2026-07-02.

## Where we are

**Phase 0 (scaffold) COMPLETE.** The seed repo `database_skills` exists locally at
`/Users/macleand/Desktop/database_skills` (git-initialised, no remote yet). It holds:

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