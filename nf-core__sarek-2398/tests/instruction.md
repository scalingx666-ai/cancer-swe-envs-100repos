# Issue

**Title:** fix: check --intervals against a user-provided FAI at start-up

## Description

## Problem

The conda shard 14/15 of the dev -> master PR (#2374) failed on `Fails with profile test --intervals intervals_wrong_contig.bed`. The pipeline failed as intended, but the output folder listing snapshot had an extra `preprocessing` entry. `BEDTOOLS_SORT` fails while BWA alignment tasks are already running, and whether they publish before the run is cancelled depends on timing. Conda sets up environments slowly, so it shows up there first.

## Change

- Check `--intervals` (BED) against a user-provided `--fasta_fai` in `validateInputParameters()`, before any process is submitted. The check also reports contigs missing from the FAI and rows with fewer than 3 columns.
- `checkIntervalsWithinContigs()` moves to `utils_nfcore_sarek_pipeline` and stays a single function. `PREPARE_INTERVALS` still calls it after `BEDTOOLS_SORT`, but only when the pipeline generates the FAI (`fai_checked` is false).
- The start-up check is skipped for `--step annotate` and `--step controlfreec`, which do not use intervals.
- Snapshots for the wrong-contig and past-end failures are regenerated: nothing is published before the failure anymore.
- New test for the generated-FAI path. That path can still race, so the scenario sets a new opt-in `ignoreFileListing` key in `UTILS.groovy` (it also ignores `Failed to render` warnings). Other failure scenarios keep their file listing check.

## Testing

Run locally on Nextflow 25.10.4 with the docker profile: the wrong-contig, past-end and generated-FAI scenarios pass, and the `--no_intervals` / intervals stub scenarios run to completion. Not verified locally:
- The `WARN` entries of the regenerated snapshots (my sandbox does not capture Nextflow stdout for successful runs, so the stub scenarios differ from their snapshot only in the `WARN` lines). Please check on CI.
- The conda profile itself (no conda cache in my sandbox).

## PR checklist

- [x] This comment contains a description of changes (with reason).
- [x] If you've fixed a bug or added code that should be tested, add tests!
- [ ] Make sure your code lints (`nf-core pipelines lint`).
- [ ] `CHANGELOG.md` is updated.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1d46c0274f2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.