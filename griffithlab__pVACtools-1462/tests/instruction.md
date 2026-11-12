# Issue

**Title:** Respect IMMU_SCOPE_WEIGHTS_DIR as first priority for determining ImmuScope weight directory

## Description

This PR needs to be merged in conjunction with 

Currently, the logic in `resolved_immuscope_weights_dir` doesn't respect/take into account `IMMU_SCOPE_WEIGHTS_DIR`. Instead only `XDG_DATA_HOME` and the default `~/.local/share'/ImmuScope/weights` are used. This PR updates the logic to first look at `IMMU_SCOPE_WEIGHTS_DIR`. `IMMU_SCOPE_WEIGHTS_DIR` is already the first priority for the weights directory when running the ImmuScope predictor via the `immuscope_wrapper.py `. After merging in  this env variable will also be the first one to be used for downloading the weights. 

With this change in these two PRs the `IMMU_SCOPE_WEIGHTS_DIR` will now be consistently respected for downloading the weights and running ImmuScope inside of pVACtools.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f9762a92a69`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.