# MEMORY — durable facts & gotchas

Committed, in-repo memory (travels with the branch). Durable conventions and hard-won gotchas — not
session chatter. Much of this is inherited from the allied stats-book migration (`../intro_to_stats`).

## Who / working style
- **Author:** Dan MacLean, TSL Bioinformatics. Sole maintainer; these are live, in-use MSc course
  materials. Be conservative about anything readers see; confirm before live-facing changes.
- **Migrate VERBATIM; flag, don't silently "fix."** A pilot on the stats book silently changed a value
  and the author (rightly) caught it. Surface content quirks for the author's call. Ping as you go.

## This project's defining constraint
- **No R. At all.** The source `learnr` tutorials use only `question()` MCQs — no code exercises, no
  computation, no data, no images. So this book is pure Quarto markdown + the `naquiz` Lua filter:
  **no `renv`, no R install, no webR/quarto-live, no `itssl`, no r-universe.** This is the whole reason
  the migration is easy. The one inline R expression in the genome source (`round((531/591)*100,2)`)
  is replaced with the literal `89.85`. If anyone reaches for webR here, stop — it's not needed.

## naquiz (the one extension)
- Vendored at `_extensions/nareal/naquiz`, listed under `filters:` in `_quarto.yml`. Question syntax:
  `:::::{.question}` (5 colons) → `::::{.choices}` (4) → `:::{.choice}` / `:::{.choice .correct-choice}`
  (3). One correct choice per question.
- **CSS scope fix is VENDORED and must be preserved.** Upstream `buttons.css` shipped
  `[id^="no"] { color: red }` (for wrong-answer badges), which also matched any element whose id
  starts with "no" — Quarto auto-generates section ids from headings, so a heading like "Not all…"
  turned whole sections red. Fixed by scoping to `.choices [id^="no"]` / `.choices #correct`.
  **Re-running `quarto add nareal/naquiz` overwrites this** — don't; if you must, re-apply the fix.
- naquiz shows **no per-answer feedback** — that's why each question gets a collapsible "Why?"
  `callout-note` (we're adding these fresh; the source had none).
- **Debugging rendered-page look (red text/layout):** inspect the real rendered DOM/computed styles
  in a browser, don't theorise. The stats-book scratchpad had a Playwright harness (`find-red.mjs`)
  that renders → serves `docs/` → reads computed colours; reuse that pattern if CSS bleed recurs.

## Hosting / deploy
- Target: `danmaclean/database_skills` on GitHub → `gh-pages` branch serves the site at
  `danmaclean.github.io/database_skills`. `publish.yml` rebuilds `gh-pages` from `master` on push.
  **Never commit `docs/`** (git-ignored). Don't hand-edit `gh-pages` (CI-generated).
- **Pages-source flip is a manual UI step.** The author's working fine-grained PAT lacks Pages-admin
  scope, so setting Settings → Pages → source = `gh-pages` must be done in the browser (learned on
  the stats book). Also: a fine-grained PAT scoped to *selected* repos can *create* a repo but can't
  push to a brand-new one until it's added to the token's allow-list — commit the first file via the
  web UI or add the repo to the token first.
- Source repos `TeamMacLean/transcriptome_dbs`, `genome_dbs`, `structural_dbs` are on branch `main`,
  deployed via `rsconnect::deployDoc`. They are **read-only inputs** here; keep them live until the
  new site is signed off, then retire.

## Shell / environment (inherited conventions)
- **Don't prepend `export PATH=…`** to Bash calls — the login PATH already resolves git/gh/quarto.
- **Don't wrap R (or `~`) in bash variable assignments** — a `~` trips tilde-expansion permission
  prompts. (Rarely relevant here since there's no R.)
- **gh** is authed as `danmaclean`. The PAT **cannot rerun workflows** (no Actions:write) — re-trigger
  via an empty commit. See the Pages/selected-repos caveat above.
- Prefer absolute paths / `git -C <dir>` over `cd` (compound `&&` chains prompt if any segment isn't
  allowlisted).