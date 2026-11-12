# Issue

**Title:** [CTM-747] Dont call python3 inside cloud-sdk image

## Description

`cloud-sdk:alpine` doesn't ship with a system python3 anymore ([5.89 release notes](https://docs.cloud.google.com/sdk/docs/release-notes#google_cloud_cli_5)), so updating this command to not rely on it anymore. This should allow Cromwell to work properly when configured to use newer versions of `cloud-sdk:alpine` (which some of our tests already do). 


Some discussion related to this change .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `03bc7eedcf95`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.