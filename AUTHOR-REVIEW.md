# Author review — database_skills conversion

Consolidated from the three per-chapter `FLAGS-*.md` reports. This is the single document to review
before go-live. Two parts: **(1) content decisions** (things the source got wrong or that will age),
and **(2) new "Why?" explanations to science-check** (all written fresh during conversion — the source
questions had no feedback).

Nothing here blocks rendering — the book renders clean end-to-end. These are editorial calls for you.

---

## 1. Content decisions needed

### Genuine source bugs (a fix is probably wanted)

| Chapter | Location | Issue | What the agent did | RESPONSE |
|---|---|---|---| --- |
| transcriptomes | `dload-2` ("reads downloaded for 8h?") | Source marked **two** answers `correct=TRUE` (`10,691,872` **and** `9,339,062`). `9,339,062` is the 14h count. | Marked only `10,691,872` correct; left the other as a distractor. **Confirm.** | CONFIRMED OK |
| transcriptomes | `taxid-1` ("what strain of E. coli?") | Correct answer is `"K12f"` — almost certainly a typo for `"K12"`. | Kept `K12f` verbatim (flag-don't-fix). **Decide: fix to `K12`?** | FIX TO K12 |
| structures | `## Aims` | Copy-paste leak from the transcriptome tutorial: "…we carry out expression estimates for the data we downloaded" — wrong for a structures tutorial. | **Corrected** to a structures-appropriate sentence (agent's call, since plainly wrong). **Confirm or reword.** | CONFIRMED OK|
| structures | Visualising section prose | Phyre best hit written as `dda321` in one instruction line; it's `d3d32a1` everywhere else. | Left verbatim. **Likely a typo to fix.** | FIX to d3d32a1 | 

### Cosmetic fixes already applied (FYI, no decision needed)

- transcriptomes `geoacc-3`: stripped a literal leading tab in a distractor.
- transcriptomes `geoacc-5`: dropped a stray trailing comma in an answer.
- transcriptomes `taxid-2` prose: "browswer" → "browser"; minor phrasing smoothing.
- genomes: the one inline R expression `` `r round((531/591)*100,2)` `` → literal `89.85` (R-free rule).
- genomes `annot-1`: numeric `answer(38)` rendered as plain `38`.

### Longevity / data-drift caveats (no fix now; note for future maintenance)

Several "correct" answers are **live database facts that grow/change over time**, so they will
eventually be wrong. Stable identifiers (taxon IDs, accessions) are fine; counts and sizes drift.

- transcriptomes `ensembl-1` ("about 1,200" E. coli genomes), `ensembl-2` ("about 26" K-12 genomes).
- genomes `cc-assembly-count-1` ("about 250" assemblies); read-count/size answers (`dload-*`).
- structures: best-hit ID, 57% identity, TM-scores, helix/residue counts — depend on what
  PDBe/Phyre2/Robetta return today or on reading a 3D viewer.
- structures **stale tool URL**: TM-align is pointed at the old `zhanglab.ccmb.med.umich.edu` host;
  the current server is generally `zhanggroup.org/TM-align/`. Kept verbatim — worth updating.
- structures: Phyre2 / Robetta / `phyreresults` links are live external resources that can move.

---

## 2. New "Why?" explanations to science-check

Every question (37 total: 12 + 14 + 11) gained a fresh collapsible **"Why?"** note. All are new prose
in the author's voice — please skim for scientific accuracy. Full per-question lists are in the
`FLAGS-*.md` files; the two the agents specifically flagged as worth double-checking:

- **transcriptomes, Q5 (E. coli K-12 taxon ID):** the note states the *species* E. coli taxon ID is
  **`562`** alongside the K-12 strain ID `83333`. Please confirm `562`.
- **structures, `pdbe-*` / `tm-align-*`:** the notes assert specific structural facts (ubiquitin-like
  fold = 2 helices, C-terminal serine, TM-score interpretation thresholds) — a quick expert eye.

The rest are straightforward database-mechanics explanations (accession-prefix conventions, what
`fastq-dump`/`grep -c '>'` report, BUSCO completeness denominator, etc.).

---

*Sources are read-only originals at `../transcriptome_dbs`, `../genome_dbs`, `../structural_dbs`
(TeamMacLean, branch `main`). Keep the shinyapps versions live until this is signed off.*
