# Issue

**Title:** Carry [meta, vcf, tbi] through POST_VARIANTCALLING (review follow-up)

## Description

Stacked on #2377. Addresses the `post_variantcalling` review point on #2374.

- **VCF and index travel together:** VCFs and their indexes are joined once at the top (`failOnDuplicate`/`failOnMismatch`, keyed on meta plus file name because somatic Strelka and Manta emit two VCFs per sample). They stay as `[meta, vcf, tbi]` through filtering, normalization, consensus, concatenation and Varlociraptor and are split into `vcfs`/`tbis` only at `emit:`. Every step, including the `publish_dir` assignment, now exists once instead of separately for VCFs and indexes.
- **Consensus:** the long `publish_dir` ternary is gone. Consensus VCFs no longer set `publish_dir`; `csv/variantcalled.csv` falls back to `variantcaller: 'consensus'`, which is the same folder.
- **Unchanged:** the subworkflow inputs and outputs, `workflows/sarek.nf`, the publish configs, the published files and the restart CSV.

`meta.publish_dir` itself stays. It never sets where files are published (`conf/modules` does); it only tells `CHANNEL_VARIANT_CALLING_CREATE_CSV` which folder to write into `variantcalled.csv` (#2360, #2204). Nextflow can't read back a `publishDir` path, so removing it fully needs workflow output definitions, which is out of scope for 3.12.

## Testing

- `nextflow lint`: no errors.
- All 11 `postprocess_*` nf-tests (filtering, normalization, concatenation, concatenation+normalization, consensus, Varlociraptor) pass with unchanged snapshots (`NXF_VER=25.10.4`).
- The snapshots don't cover the restart CSV, so the same scenarios were also run against #2377's head: `csv/variantcalled.csv` is byte-identical and the published file lists match in all 11.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4352d313ea04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.