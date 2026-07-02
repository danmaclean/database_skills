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

**▶ Remaining follow-ups (non-blocking):** optional in-browser click-test of naquiz on the live pages;
future maintenance of the drift-prone answer counts flagged in `AUTHOR-REVIEW.md`. (The three
shinyapps.io tutorial deployments have been **undeployed** by the author, and the old
`danmaclean/databases` sub-book repo is **archived** — both done 2026-07-02.)

> **Incident/lesson (2026-07-02):** a stray no-op `fork` launched during scaffolding inherited the
> full plan and autonomously ran the whole conversion *in parallel* with the intended agents, doing
> git surgery (branch moves, an amend) that tangled history and briefly looked like data loss.
> Nothing was lost; history was rebuilt cleanly off `master`. **Lesson: never leave a `fork` running
> with a vague/no-op prompt — it will act on your whole context. Kill stray agents immediately.**

## Course integration & parity (done 2026-07-02, after go-live)

`database_skills` now replaces the old `databases` sub-book everywhere in the course:

- **Folded the `databases` sub-book text into this book** (branch `add-course-text`, merged to `master`):
  new `00-primary-secondary.qmd` background chapter (verbatim from `databases/02-primary-secondary.qmd`),
  prerequisites (tool installs + Colab) added to the `index.qmd` preface, and an "About this chapter"
  Questions/Objectives opener prepended to each tutorial. Old shinyapps-link callouts dropped.
- **Repointed the two course hubs** at `https://danmaclean.github.io/database_skills/`:
  - `danmaclean/data_science` (main MSc hub) — link in `genomics.qmd`.
  - `danmaclean/data_science_tsl` (institute variant) — link in its own `onlinedatabases.qmd` chapter.
  Both hubs serve from **committed `docs/`** (no CI) and have **executable R chapters**, so rather than a
  full renv render (which churns every page's `last-modified` date) the URL was swapped **surgically** in
  three places each: the source `.qmd`, the built `docs/*.html`, and Quarto's `idx/*.json` cache. Both live.
- **Removed dead chatbot sidebar links from `data_science_tsl`** (parity with `data_science`, which had
  already dropped them): four `other-links` chatbots on the defunct `tsl-bioinformatics.shinyapps.io`.
  Removed the `other-links` block from `_quarto.yml` and stripped the one-line
  `<div class="quarto-other-links">…</div>` sidebar block from all 11 committed `docs/*.html`. Live.
- **Originals retired:** author undeployed the three shinyapps tutorials; `danmaclean/databases` archived.
  Residual: the two hubs' downloadable **PDF/EPUB** still show the old DB URL until their next full render
  (cosmetic; chatbot links were HTML-sidebar-only so never in the PDFs).

## Key facts / maintenance notes

- **This book has no R** — pure Quarto + naquiz. Don't add webR/renv/knitr. Don't re-`quarto add` naquiz
  (clobbers the vendored `.choices`-scoped CSS fix; see `MEMORY.md`). Migrate-verbatim/flag-don't-fix and
  the known content quirks are in `SPEC.md` / `AUTHOR-REVIEW.md`.
- **`master` = trunk + live source**; `publish.yml` republishes `gh-pages` on every push. Never commit `docs/`.
- **Committed-docs hubs (`data_science`, `data_science_tsl`):** to change a link without a heavy render,
  edit source `.qmd` + `docs/*.html` + `idx/*.json` together (the surgical pattern used here).
- **Drift-prone answers** (live DB counts) are catalogued in `AUTHOR-REVIEW.md` for a future refresh pass.

---
*History: scaffolded (Phase 0) → 3 chapters converted via parallel agents (Phase 1) → integration/QA
(Phase 2) → author review + go-live (Phase 3) → course integration/parity (above). The conversion agent
prompts remain in `.claude/prompts/` for reference.*