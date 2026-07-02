# ROADMAP — Finding and Using Biological Databases

Planning document. The live status lives in `NEXT-SESSION.md`; the conversion contract in `SPEC.md`.

## Origin & goal

Three `learnr`/Shiny tutorials on shinyapps.io (`TeamMacLean/transcriptome_dbs`, `genome_dbs`,
`structural_dbs`) that drill learners on using public transcriptome / genome / structural databases.
**Goal:** consolidate them into one static Quarto book that runs entirely in the browser, killing the
Shiny server dependency and its hosting cost, while keeping the interactive self-check questions.

This mirrors the successful migration of the allied stats book (`../intro_to_stats`), but is far
simpler: those tutorials had `learnr` code exercises that needed webR; **these are pure multiple-choice**,
so no webR/R stack is required at all.

## Decisions taken (session 2026-07-02)

- **One combined book**, not three separate sites. Title *"Finding and Using Biological Databases"*,
  repo `danmaclean/database_skills`, served at `danmaclean.github.io/database_skills` via `gh-pages`.
- **Quarto + `naquiz` only. No webR, no R, no `renv`.** The content is prose + MCQs; keep it R-free.
- **MCQs are enriched** with a short collapsible "Why?" explanation each (the source has none).
- **Migrate verbatim, flag don't fix** — same author policy as the stats book.
- Chapters keep their own `## Aims` opener (not the stats book's Questions/Objectives/Keypoints).

## Phases

### Phase 0 — Scaffold ✅ (done this session)
Repo skeleton, `_quarto.yml` (flat 3-chapter book), vendored naquiz (with CSS fix), R-free CI
(`render.yml` + `publish.yml`), `.gitignore`, README, preface (`index.qmd`), chapter stubs, and this
context pack. Skeleton renders clean.

### Phase 1 — Convert the three chapters (downstream agents; parallel)
One agent per chapter, following `SPEC.md` and its `.claude/prompts/agent-*.md`. Each produces the
converted `.qmd` + a `FLAGS-<chapter>.md` report. Chapters are independent — run them in parallel.

### Phase 2 — Integrate & QA (one agent)
Full `quarto render`, rendered-DOM check for naquiz CSS bleed, consolidate all FLAGS into one
author-review document, tidy nav/cross-links. Output: a book that renders clean end-to-end.

### Phase 3 — Author review & go-live (human + assistant)
Author reviews the consolidated FLAGS (content bugs + the new "Why?" prose) and signs off. Then:
create the GitHub repo `danmaclean/database_skills`, push `master`, let `publish.yml` build `gh-pages`,
set Pages source to `gh-pages` (manual UI step — see MEMORY re the Pages-admin token limit), verify
the live site. Keep the shinyapps versions live until sign-off, then retire them.

## Out of scope (candidate follow-ups)

- Refreshing drifted answer counts (live-DB facts) — a maintenance pass, not migration.
- Adding screenshots/figures of the database UIs (the source has none; would be new content).
- Any real runnable code (e.g. a webR kallisto/quant demo) — deliberately excluded; revisit only if
  the author wants to add genuine compute later.