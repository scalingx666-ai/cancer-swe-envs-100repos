# Issue

**Title:** Address review nits from the 3.28.0 release PR

## Description

## Summary

Addresses the non-blocking nits from the review on the 3.28.0 release PR (#1996).

- `.nf-core.yml`: drop the comment saying `nf_core_version` is held below the template version. It is already 4.2.1.
- `main.nf`: `anySampleAutoStrandedness` reads `row[0].strandedness` instead of destructuring all five `schema_input.json` columns, so adding a column cannot cause an argument-count error at startup.
- `CHANGELOG.md`: complete the 3.28.0 software dependencies table with `htslib` (1.24 to 1.25), `parabricks` (4.7.1-1 to 4.8.0-1), and the bowtie2 modules' own `htslib`/`samtools` bumps (1.21 to 1.24). The versions were checked against the `environment.yml` and container diffs since 3.27.0.

No functional change beyond the `main.nf` closure.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ae2ca5c5e8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.