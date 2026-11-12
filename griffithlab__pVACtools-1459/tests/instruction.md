# Issue

**Title:** Fix issue in the Transcript Pass logic that would cause pVACview to crash on retiering

## Description

Using `apply(df$mainTable, TRUE ...` instead of `apply(df$mainTable, 1 ...` when calculating `transcript_pass` status would cause pVACview to crash during re-tiering. This PR fixes that. It also centralizes the logic for this status by re-using the existing `is_transcript_pass` function. Additionally, the exiting logic to add a new column for the results of this function during retiering would result in a second Transcript Pass column to be appended instead of the existing column to be updated. This PR fixes that.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f9762a92a69`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.