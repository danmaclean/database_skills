# FLAGS — `01-transcriptomes.qmd`

Conversion of `../transcriptome_dbs/transcriptomes.Rmd`. Per author policy, content quirks are
surfaced here (and marked with an HTML comment at the point of the quirk) rather than silently fixed.

## Content quirks flagged

- **`dload-2` — "How many reads were downloaded for 8h?" marks TWO answers correct.**
  Source: `answer("10,691,872", correct=TRUE), answer("9,339,062", correct=TRUE)`.
  `9,339,062` is the 14h count (it is the correct answer to `dload-1`), so the intended 8h answer is
  `10,691,872`. **Action:** marked only `10,691,872` as `.correct-choice`; left `9,339,062` as a
  distractor. HTML comment left in place. Author should confirm and remove the stray `correct=TRUE`.

- **`taxid-1` — "What strain of E. coli was used?" correct answer is `"K12f"`.**
  Almost certainly a typo for `"K12"` (the K-12 lab strain; consistent with `taxid-2`, which asks for
  the K-12 strain taxon ID). **Action:** kept the source string `K12f` verbatim as the correct choice
  per flag-don't-fix policy; HTML comment left in place. Author should decide whether to fix to `K12`.

- **`geoacc-3` — distractor had a literal leading tab.**
  Source: `answer("\tSRP069289")`. **Action:** stripped the tab (cosmetic); renders as `SRP069289`.

- **`geoacc-5` — trailing comma in an answer.**
  Source: `answer("SRR3144643",)`. **Action:** dropped the stray trailing comma (cosmetic); text
  unchanged.

- **`taxid-2` — typo in the question prose.** Source read "NCBI taxonomy browswer". **Action:**
  lightly corrected to "browser" (obvious typo, per SPEC's allowance to fix obvious typos). Also
  smoothed "the taxonomy ID *E.coli* K12" → "the taxonomy ID of *E. coli* K12". Flagging for the
  record.

- **Longevity / data-drift risk (whole chapter).** Two answers are live database result counts that
  change over time, not stable identifiers:
  - `ensembl-1` — "about 1,200" *E. coli* genomes in EnsemblBacteria.
  - `ensembl-2` — "about 26" *E. coli* K12 genomes.
  These figures grow as new assemblies are deposited and may become wrong. Taxon IDs and accessions
  (`GSE77674`, `GSM2055958`, `SRR3144708`, `SRR3144643`, `83333`, `gca_004802935`) are stable. No fix
  applied — noting the maintenance risk for the author.

## New explanations to review

Every `.question` gained a fresh collapsible `## Why?` `callout-note` (the source had no feedback).
These are newly written prose — please science-check:

1. **How many samples in the Series?** — GSE groups samples; two time points ⇒ two GSM records.
2. **What sequencing machine?** — platform recorded in sample metadata (Illumina HiSeq); why platform
   matters (read length / error profile differ across 454, Illumina, Nanopore).
3. **Which accession describes the 8h reads?** — samples carry `GSM` accessions; `SRP069289` is an SRA
   *Study* accession (whole project), not a sample.
4. **What strain of E. coli?** — metadata names the K-12 lab strain; DH5a/W3110 are other lab strains.
5. **Taxonomy ID of E. coli K12 (strain level)?** — `83333` is the K-12 strain node; species *E. coli*
   is `562`. (I state `562` as the species ID — please confirm.)
6. **How many replicates per sample?** — one run per time point ⇒ one replicate; statistically weak
   but keeps the demo small.
7. **Run id for the 8h sample?** — reads live under a Run (`SRR`) accession; `SRX...` is an
   Experiment/library accession.
8. **Run id for the 14h sample?** — same `SRR` vs `SRX` distinction; 14h reads under `SRR3144643`.
9. **Reads downloaded for 14h?** — `fastq-dump` reports reads/spots written; 9,339,062 for
   `SRR3144643`; count is a fixed property of the run.
10. **Reads downloaded for 8h?** — 10,691,872 for `SRR3144708`; 9,339,062 is the 14h count and a
    tempting distractor.
11. **How many E. coli genomes in EnsemblBacteria?** — heavily sequenced organism ⇒ ~1,200 assemblies;
    figure grows over time.
12. **How many E. coli K12 genomes?** — filtering by the K-12 taxid narrows to a couple dozen;
    filtering on a stable taxid is the reliable way to pick a strain.
