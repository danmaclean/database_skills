# Agent prompt — integration & QA

You are the integration/QA agent working in `/Users/macleand/Desktop/database_skills`.
Run this **after** the three conversion agents have produced `01-transcriptomes.qmd`,
`02-genomes.qmd`, and `03-structures.qmd` plus their `FLAGS-*.md` reports.

## Read first
`CLAUDE.md`, `.claude/SPEC.md` (esp. "Verification"), `.claude/MEMORY.md` (naquiz CSS-bleed lesson).

## Do
1. **Full render.** `quarto render` from the repo root → must exit 0 with no error. Fix any
   book-level issues (broken cross-refs, nav, malformed fenced-divs). If a chapter itself is broken,
   don't rewrite its content — report it back for that chapter's agent to fix.
2. **naquiz sanity across all chapters.** Confirm every `.question` block is well-formed (5/4/3 colon
   nesting), has exactly one `.correct-choice`, and is followed by a `## Why?` callout. Grep all three
   chapters for stray `{r`/`{webr`/`{ojs` — there must be none (the book is R-free).
3. **CSS-bleed check (the known naquiz gotcha).** Verify `_extensions/nareal/naquiz/css/buttons.css`
   still has the `.choices`-scoped selectors (`.choices [id^="no"]`, `.choices #correct`) — not the
   bare `[id^="no"]`. Then check the *rendered* pages: serve `docs/` (`python3 -m http.server` in
   `docs/`) and confirm no section heading/body text is wrongly coloured red/green (a browser/DOM
   check, not a guess — see MEMORY for the Playwright pattern). Reds/greens must appear only inside
   answered `.choices`.
4. **Consolidate flags.** Merge the three `FLAGS-*.md` into one `AUTHOR-REVIEW.md` at the repo root
   with two sections: **"Content quirks needing an author decision"** (each: chapter, location,
   issue, what the agent did) and **"New 'Why?' explanations to science-check"** (grouped by chapter).
   This is the single document the author reviews before go-live.
5. **Tidy.** Ensure `docs/` is not staged (git-ignored). Confirm `index.qmd` preface still reads well
   against the three finished chapters.

## Deliverables
- A book that renders clean end-to-end (`quarto render` exit 0), naquiz working, no CSS bleed.
- `AUTHOR-REVIEW.md` consolidating all flags + new explanations.
- A short summary back to the human: render status, any chapter bounced back, and a pointer to
  `AUTHOR-REVIEW.md`.

## Don't
- Don't rewrite chapter teaching content or resolve flagged content bugs yourself — that's the
  author's call; your job is to surface them cleanly.
- Don't add R/webR/renv. Don't `quarto add nareal/naquiz` (clobbers the CSS fix). Don't commit `docs/`.
- Don't create the GitHub repo or flip Pages settings — that's the Phase-3 human/assistant step
  (see ROADMAP; the working PAT lacks Pages-admin).
