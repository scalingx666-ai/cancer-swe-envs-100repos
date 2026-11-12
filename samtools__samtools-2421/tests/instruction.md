# Issue

**Title:** Fix null pointer exception in "head" when given malformed headers.

## Description

Eg
    samtools head -- 'data:;base64,QFhYCXkKCg=='

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `16f19c7b18fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.