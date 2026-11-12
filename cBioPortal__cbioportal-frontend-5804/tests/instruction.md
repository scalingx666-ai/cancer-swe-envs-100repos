# Issue

**Title:** Add Normal Allele Freq column to patient view mutation table

## Description

Adds a **Normal Allele Freq** column to the patient view mutation table, next to the tumor "Allele Freq" column.

- Reads `normalAltCount` / `normalRefCount` and renders them the same way as the tumor column: one colored bar per sample with a per-sample tooltip (`0.04 (18 variant reads out of 429 total)`), or the plain number when there is a single sample.
- The column is only included when at least one mutation has normal read counts, so studies without that data are unchanged.
- It is **visible by default only when the tumor Allele Freq column has no data**, as with clonal hematopoiesis variants called in the normal sample. Otherwise it can be turned on from the Columns menu.
- A normal frequency of 0 counts as real data and is drawn as a 1px baseline, so it looks different from missing data. The tumor column's handling of 0 is unchanged.
- Implemented by giving `AlleleFreqColumnFormatter` an optional `'tumor' | 'normal'` source (defaults to tumor), with unit tests.

This matters most for clonal hematopoiesis variants. There the signal is in the normal (blood) samples, and today it only shows up as raw "Variant Reads (Normal)" / "Ref Reads (Normal)" text.

## Preview

- Multi-sample, per-sample normal bars: https://deploy-preview-5804.preview.cbioportal.org/patient?studyId=lgg_ucsf_2014&caseId=P04
- Clonal hematopoiesis: https://deploy-preview-5804.preview.cbioportal.org/patient?studyId=msk_ch_2023&caseId=P-0000037

Both public examples have tumor allele frequencies, so turn on "Normal Allele Freq" from the Columns menu. The column is on by default for studies with no tumor read counts, such as the internal MSK IMPACT CH variants study.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_01SFKQgm7Saqbzgd4i2zm2pz

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c6c8b92d579b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.