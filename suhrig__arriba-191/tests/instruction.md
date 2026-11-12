# Issue

**Title:** feat: keep exons while converting

## Description

The latest versions of arriba comes with a separate script for annotating exons which is really nice.
However, when utilizing the `convert_fusions_to_vcf.sh`-script exon information will be lost in that process.
To preserve exons during conversion I modified the script checking if the `exon_number1` column exists.
In case it does an additional INFO-entry called `EXON` will be appended to the already existing `INFO`-field.

I assumed that if exons are annotated `exon_number1` and `exon_number2` will always be in columns 31 and 32.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1de5d6ff223f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.