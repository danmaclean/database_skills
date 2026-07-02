# FLAGS — `03-structures.qmd` (source: `../structural_dbs/structural_databases.Rmd`)

## Content quirks flagged

- **`## Aims` copy-paste leak (CORRECTED — please confirm).** The source Aims paragraph ended
  "We shall then work through an example in which we carry out expression estimates for the data we
  downloaded." That sentence is lifted from the transcriptome tutorial and is wrong for a structures
  tutorial (there are no expression estimates here). Because it is plainly wrong, I replaced it with:
  "We shall then work through an example in which we search for a known structure that fits a protein
  of interest and predict the structure of a protein of our own." An HTML comment marks the spot in
  the chapter. Revert or reword if you'd rather keep the original.

- **Stale TM-align host (longevity caveat — NOT changed).** The source points at
  `https://zhanglab.ccmb.med.umich.edu/TM-align/` for TM-align. The Zhang Lab moved from the Michigan
  (`zhanglab.ccmb.med.umich.edu`) host some years ago; the current TM-align server is generally at
  `https://zhanggroup.org/TM-align/`. I kept the source URL verbatim per the flag-don't-fix policy —
  worth updating on a content pass.

- **Other tool URLs may drift (longevity caveat).** Phyre2
  (`http://www.sbg.bio.ic.ac.uk/phyre2/...`), Robetta (`https://robetta.bakerlab.org/`) and the
  pre-computed result links (`danmaclean.github.io/phyreresults`,
  `raw.githubusercontent.com/danmaclean/phyreresults/.../robetta_models_89846.pdb`) are all live
  external resources that can move or go offline. Kept verbatim; note as a maintenance risk.

- **Answer values are live/interpretive database facts (longevity caveat).** Several correct answers
  depend on what the databases return today or on reading a 3D viewer (best-hit ID `5l83`, 57% Phyre2
  identity, TM-score `0.87/0.92`, helix/residue counts). These can drift as PDBe, Phyre2 and Robetta
  update. IDs and accessions are stable; counts and scores less so.

- **Two-option questions (preserved, not a bug).** `tm-align-1` ("Is the alignment a good one?",
  Yes/No) and `tm-align-2` (TM-score, two options) each have only two choices; `tm-align-3` has four.
  All preserved exactly as in the source.

- **Typo in the walkthrough prose (NOT changed).** The Visualising section refers to the Phyre best
  hit as `dda321` in the instruction text, whereas it is `d3d32a1` everywhere else (and in the Phyre2
  questions). Left verbatim; likely a typo the author may want to fix.

## New explanations to review

All "Why?" callouts are new prose written for this conversion — please science-check.

1. **pdbe-1 (best-hit ID `5l83`):** notes `5l83` is a four-character PDB accession, while the
   distractors resemble a UniProt accession / a numeric ID.
2. **pdbe-2 (potato ATG8 in complex with PexRD54):** the hit is a complex, so it contains both the
   ATG8 protein and the PexRD54 effector rather than either alone.
3. **pdbe-3 (2 helices):** the larger molecule shows two helices against the central beta sheet — the
   ubiquitin-like fold of ATG8.
4. **pdbe-4 (Serine):** following the final beta strand to its C-terminal end lands on serine; the
   viewer lets you hover to confirm residue identity.
5. **pdbe-5 (ATOM most common):** one line per atom means `ATOM` records dominate; `REMARK`/`SITE` are
   sparse metadata.
6. **pdbe-6 (ATOM = atom coordinates):** `ATOM` records store x/y/z coordinates; authorship etc. lives
   in header records like `AUTHOR`.
7. **phyre-1 (57% identity):** low sequence identity can still give a high-confidence homology model
   because the fold is mapped, not just the sequence.
8. **phyre-2 (0 extra features):** every predicted sheet/helix in the query matches one in the known
   structure, hence zero extras and high confidence.
9. **tm-align-1 (Yes):** the structures superpose closely, so the alignment is good and supports
   inferring our protein's shape.
10. **tm-align-2 (`0.87/0.92`):** TM-score is a 0–1 value (normalised two ways); `122/155` is the
    aligned-residue count, not the score; >~0.5 implies same fold.
11. **tm-align-3 (0.3):** random alignments of unrelated structures top out near 0.3, so scores above
    that indicate genuine similarity; our ~0.9 is well beyond it.
