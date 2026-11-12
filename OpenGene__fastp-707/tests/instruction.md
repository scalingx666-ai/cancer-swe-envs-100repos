# Issue

**Title:** Add additional checks for non-seekable input

## Description

Pre-preocessing evaluation searches for adapters etc. by reading some of the input then rewinding the file, and this is turned off for /dev/stdin explicitly because rewinds won't work. Unfortunately, this doesn't catch process substitution inputs (e.g. `<(zcat file.fq.gz)`) which then silently drop data when the rewinds fail.

This commit introduces a new helper function which checks whether the file descriptor is rewindable, turning off evaluation if not.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f1708fdb6da4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.