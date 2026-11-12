# Issue

**Title:** feat(relate): seed the sampling of unrelated pairs from SOMALIER_SEED

## Description

`somalier relate` calls `randomize()` on every run, so above about 450 samples, where it keeps a random subset of the unrelated pairs, `somalier.html` and `somalier.pairs.tsv` differ from run to run on the same input. That gets in the way of pipelines that cache or compare their outputs.

This reads an optional `SOMALIER_SEED` and seeds the generator with it, beside the existing `SOMALIER_REPORT_ALL_PAIRS` and `SOMALIER_AB_HOM_CUTOFF` switches. Unset, nothing changes. A value that is not an integer stops with an error.

Checked with Nim 2.2.4 on 500 synthetic unrelated samples (80% of the unrelated pairs sampled): two runs with `SOMALIER_SEED=7` write byte-identical `somalier.html` and `somalier.pairs.tsv`, two runs without it differ, `SOMALIER_SEED=0` is repeatable too, and `SOMALIER_SEED=abc` exits 1 with `[somalier] SOMALIER_SEED must be an integer, got: abc`. I did not add a test, since the functional tests have no fixture with enough samples to reach the subsetting.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1ff92a7af420`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.