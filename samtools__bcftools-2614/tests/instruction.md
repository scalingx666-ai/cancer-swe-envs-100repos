# Issue

**Title:** Gentoo fixes

## Description

- Don't install plugin sources when plugins are disabled
- Skip `test_trio_dnm3` unless plugin tests are enabled

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b62c1bda5bf8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.