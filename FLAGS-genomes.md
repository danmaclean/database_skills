# FLAGS — `02-genomes.qmd`

## Content quirks flagged

- **Inline R in the BUSCO wrap-up (R-free rule, not a bug).** The source (`genomes.Rmd`, final
  paragraph before "Wrap up") contained `` `r round((531/591)*100, 2)` `` to compute the completeness
  percentage. Per the R-free rule in `SPEC.md` this was replaced with the literal **`89.85`**. No
  behaviour change; the rendered text now reads "531 / 591 BUSCOs found (89.85% )".

- **`annot-1` numeric answer.** The source distractor was `answer(38)` (a numeric, not a string).
  Rendered as the plain choice `38`, matching the other options. No content change.

- **`cc-assembly-count-1` — drifting result count (longevity risk).** The correct answer is
  "about 250" assemblies. This is a *live* ENA count that grows over time, so the "correct" option
  will eventually be wrong. Kept verbatim as in the source; flagging as a maintenance/longevity risk.
  (Same caveat applies book-wide to any answer that is a live database count, e.g. read/data sizes;
  taxon IDs and accessions such as `199`, `GCA_015679965`, `PRJNA348396`, `SRR4434292` are stable.)

- **`cc-assembly-count-2` distractor formatting.** The correct option is "approximately 1.8 Mbp";
  the distractor "1861371 Mbp" mislabels bases as megabases. Kept exactly as written in the source —
  not corrected — since the wrong units are arguably intentional as a distractor.

- **Question-label metadata dropped (expected).** Source chunk labels (`cc-taxid`, `dload-2`,
  `busco-1`, etc.) were dropped per the transform spec; naquiz needs no ids. Recorded here only so
  the mapping between source chunks and rendered questions is traceable.

No double-`correct = TRUE` questions were found in this source; each `question()` had exactly one
correct answer, so every naquiz block has exactly one `.correct-choice`.

## New explanations to review

Each `.question` gained a fresh collapsible "Why?" `callout-note` (the source had no feedback). All
new prose below is written for science-check by the author:

- **cc-taxid (199):** Every taxon has one stable, unique NCBI Taxonomy ID; 199 is *C. concisus*.
  Pinning the taxon ID first is what lets you query linked databases for exactly the right species.
- **cc-assembly-count-1 (about 250):** Searching ENA by taxon ID returns all assemblies for the
  species (a couple hundred); larger counts pull in beyond-species records; the tiny count misses
  most. Notes the count drifts over time.
- **cc-assembly-count-2 (approximately 1.8 Mbp):** The record reports ~1.8 Mb, typical for
  *Campylobacter*; "1861371 Mbp" confuses bases with megabases; "15679965 bp" echoes accession digits.
- **cc-assembly-count-3 (PRJNA348396):** `PRJ` prefix marks study/project accessions; `SAMN…` is a
  BioSample and `GCA_…` is the assembly accession itself.
- **cc-run-count-1 (Mostly Illumina Paired End):** Run metadata records platform/library layout;
  mostly Illumina paired-end is the standard short-read approach here; Sanger/PacBio aren't listed.
- **cc-run-count-2 (32):** The study run table lists each run; counting gives 32. Notes 199 is the
  taxon ID (a decoy).
- **dload-1 (399,928):** `fastq-dump` reports the number of spots/reads written (399,928); smaller
  options are truncated fragments of that figure.
- **dload-2 (1):** Plain `fastq-dump` without `--split-files` writes all reads to a single file; two
  files would require splitting paired mates.
- **dload-3 (about 51):** The uncompressed fastq is ~51 MB for ~400k short reads plus qualities;
  399,928 is the read count, not a size.
- **assemble-1 (about 65):** `grep -c '>'` counts FASTA headers, one per scaffold, giving ~65; a
  well-resolved bacterial assembly would have far fewer, but the restricted read set fragments more.
- **annot-1 (1905):** Coding sequences are the `CDS` line (1905); 68 is contigs and 38 is tRNAs from
  the same summary.
- **busco-1 (591):** `n:591` / "Total BUSCO groups searched" is the number expected for the lineage —
  the completeness denominator; 531 found, 59 missing.
- **busco-2 (531):** "Complete BUSCOs (C)" = 531 recovered of 591; 59 missing, 591 total.
- **busco-3 (Yes):** 531/591 is high completeness, good given the deliberately small read set; higher
  completeness means more expected genes were reconstructed.
