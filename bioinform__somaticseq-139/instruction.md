# Issue

**Title:** Use pybedtools

## Description

Pybedtools moved to MIT license since 0.90.0. We'll use that package instead of bedtools CLI directly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29e56c142eab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.