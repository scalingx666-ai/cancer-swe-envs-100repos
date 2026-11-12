# Issue

**Title:** Improve error handling for amplicon_stats `amps` array allocation

## Description

* Check for an error return (-1) from `sam_hdr_nref()`

* Avoid `calloc(0, sizeof(*amps))` for files with no refs

* Replace `bam_nref` with `nref` as both were set from `sam_hdr_nref(header)` and the answer should not have changed between calls.

* Don't try to iterate through amps if the `calloc()` to create it failed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9778338f45dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.