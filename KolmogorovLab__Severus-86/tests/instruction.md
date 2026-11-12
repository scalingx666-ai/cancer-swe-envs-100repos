# Issue

**Title:** Fix `--ignore-hp` VCF output crash

## Description

Passes `args.ignore_hp` into the VCF writer from `output_graphs()`, matching the updated `write_to_vcf()` signature and preventing `--ignore-hp` runs from crashing at the VCF output step.

Sorry for the bug introduced earlier. It came from me reverting whitespace changes and accidentally reverting the `write_to_vcf()` call line back to the old argument list.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1c4219bba28b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.