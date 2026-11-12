# Issue

**Title:** Fix sample freeze

## Description

Sample Freeze data were uploaded as an older version in `scripts/alternative_genes_pathwaymapper.ipynb` and `scripts/ras_count_heatmaps.ipynb`.

I update it here. I also add the updated sample_freeze file itself (removed unused columns) and a couple minor syntax updates

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9fd9afbecdbb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.