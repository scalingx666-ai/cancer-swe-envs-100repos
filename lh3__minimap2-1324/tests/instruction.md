# Issue

**Title:** Convert unmapped reads in sam2paf

## Description

Not sure what might break in paftools.js with unmapped paf reads (given the manpage text for `--paf-no-hit`), but allowing a flag to convert unmapped sam2paf is useful.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bd0cba50129c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.