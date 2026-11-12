# Issue

**Title:** fix survival label

## Description

The censorship label in certain survival CSV files has been reversed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `31fb87ee3f86`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.