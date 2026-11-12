# Issue

**Title:** refactor: remove duplicated code, apply yapf

## Description

Repeated operations are packed in functions in 1_split.py.
path handling is generalized.
Compliant with PEP8 and yapf.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `92c4eb9a4753`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.