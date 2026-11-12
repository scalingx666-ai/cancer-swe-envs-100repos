# Issue

**Title:** Address review feedback on the 3.28.0 release PR

## Description

Addresses the  by FriederikeHanssen.

- `anySampleAutoStrandedness()` ignores rows with a genome or transcriptome BAM when `--skip_alignment` is set. Strandedness is only inferred on the FASTQ path, so a Salmon index built for BAM-only `auto` rows was never used.
- `getHisat2PercentMapped` returns `null` (with a warning) when the alignment rate cannot be parsed, so the sample is not filtered on `--min_mapped_reads`, instead of returning 0 and dropping every sample. Function tests cover a match and a miss.
- The transcriptome BAM filter in `workflows/rnaseq/main.nf` names its two conditions (`has_transcriptome_bam`, `failed_mapped_check`).
- `EAUTILS_GTF2BED` conda lock paths (`conf/containers_conda_lock_files_{amd64,arm64}.config` and the module's `meta.yml`) point at `modules/nf-core/eautils/gtf2bed/` instead of the removed `ea-utils` directory. The `meta.yml` fix is nf-core/modules#13697, pulled in by a module update.
- `CITATIONS.md` links Trim Galore to its current repository, matching `docs/usage.md`.
- `CHANGELOG.md` notes that #1959 stopped pinning `--seed 1` for RSEM, and adds this PR's entry.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4897f259afc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.