# Issue

**Title:** Rework util_model

## Description

Fix inconsistent indentation across this file, simplify model instantiation function, and correct un-normalization numbers (although this doesn't affect to the main pipeline because reversing normalization is debugging purpose only and it's commented out).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2fa70fd35f02`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.