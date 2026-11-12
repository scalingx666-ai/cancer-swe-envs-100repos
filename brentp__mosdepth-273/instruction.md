# Issue

**Title:** Reduce memory use when processing intervals and coverage statistics

## Description

Two small changes to reduce memory use:

- Store BED intervals without repeating chromosome names.
- Calculate coverage statistics and thresholds without copying array slices.

On HG001 and HG002 with genome-wide 1 kb BED intervals, these changes roughly halved peak memory, with a small runtime improvement. All compared outputs matched the original.

Checked with Nim 1.6.18 and 2.0.2; the available upstream checks passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3211a79388ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.