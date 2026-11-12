# Issue

**Title:** CLI discoverability: examples, flag suggestions, bare summary, outputs list (3.49.10)

## Description

Part of #772 (items 1, 2, 3, 4).

- `--help` on all three parsers ends with worked examples (variant+BAM, pVACseq/LENS import, replay).
- Unknown long flags get `did you mean --bam?`. The check runs before parsing because argparse reports missing required flags first, which hid the typo. Unique prefixes still work.
- Bare `vaxrank` prints a short summary plus the examples and exits 0 (it used to demand `--bam --mhc-predictor`).
- A run ends with one `Outputs written:` block listing the files and construct directories that exist.

Not done from #772: the "warnings seen" and product counts in the final block, marking first-run flags in the 950-line help, and the flag-alias naming overlap. Those stay on the issue; #766 (subcommands) will shrink the help page.

Version 3.49.10. Full suite: 2001 passed; `test_oncoref_adoption::test_released_species_cta_snapshots_keep_evidence_and_offline_identity` failed once mid-suite and passes alone (unrelated to this diff, will confirm on CI).

https://claude.ai/code/session_01JHJhwgWvLjF45BipXVgYXg

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `71809869e116`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.