# Issue

**Title:** Fix always falling back to non-bgzip reader && allow .bgz extension

## Description

Previously, files with .gz extension were assumed to be bgzip files.
Now, `.gz` is assumed gzip and `.bgz` is assumed bgzip.

I think it makes more sense this way, and hope the author agrees.

EDIT: This is not an adequate assumption, but I discovered that bgzip reader was never being used and is fixed below.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `54bcc00049d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.