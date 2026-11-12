# Issue

**Title:** fix bug for fits with > 25 sigs

## Description

- patch that fixes bug in code for mutational signature refitting; in cases where number of contributing signatures > 25.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2e223ac650b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.