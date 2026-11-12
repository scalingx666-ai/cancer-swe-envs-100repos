# Issue

**Title:** Add `--ignore-hp` option

## Description

- Adds a new `--ignore-hp` CLI option that ignores BAM `HP`/`PS` tags during read parsing and coverage collection, treating the run as unphased end-to-end. 
- Updates breakpoint, graph, and VCF output paths so phased outputs are suppressed when this mode is enabled, and wires the new flag through the remaining VCF call path.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3dc316d6a3c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.