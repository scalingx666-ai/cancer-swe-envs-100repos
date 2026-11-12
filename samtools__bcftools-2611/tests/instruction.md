# Issue

**Title:** sort: preserve 64-bit values and detect temporary-file I/O errors

## Description

Fixes assertion failures and coordinate truncation when sorting VCF records with positions beyond the signed 32-bit range.

Also fixes detection of temporary-file read/write errors

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `289397747dec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.