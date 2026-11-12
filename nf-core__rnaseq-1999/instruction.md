# Issue

**Title:** Declare force_completion_email and test remaining fail_on params

## Description

Follow-up to the review on the 3.28.0 release PR (#1996).

- Declare `force_completion_email` (default `false`) in `nextflow.config` and the schema. The `nf-core-utils` plugin reads it, but it was undiscoverable and nf-schema flagged it as unrecognised.
- Test `--fail_on_low_trimmed_reads` and `--fail_on_low_mapped_reads`, next to the existing strandedness test. Passes with docker on x86_64.
- Fix the `strandCheckSummaryYaml rejects rows with undeclared columns` test, which read a nonexistent `function.errors` and threw under Nextflow 26.10. It now checks `function.stderr`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f10fb931fa9e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.