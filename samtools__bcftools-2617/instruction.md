# Issue

**Title:** Saturate --max-idepth instead of overflowing when scaled by sample count

## Description

As of right now, `mpileup` scales the per-sample `--max-idepth` by the number of samples in an `int`.

With a large value and an even number of samples the product can become negative! This results in `total_depth < max_indel_depth` becoming never true, and indel calling is then silently skipped at every site.

I hit this calling variants per cell on single-cell data, where each cell is its own read group in one BAM (14,382 samples). For example, `-L 2147483647` dropped every indel, while the default of `-L 250` retained the indels.

The fix in this PR multiplies in 64 bits and clamps to `INT_MAX`.

This PR also updates the usage text to describe -L as a per-sample depth, matching the manual.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0b732b9682cb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.