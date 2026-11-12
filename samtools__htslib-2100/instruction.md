# Issue

**Title:** Use pos=-1 for CRAM query of region "*".

## Description

In CRAM 3.0 our containers had pos 0, span 1, meaning start=0 end=0 and our index worked.  With our CRAM 3.1 output this was changed, to be spec compliant and use pos 0, span 0, but unfortunately it broke searching on region "*".

Fixes samtools/samtools#2396

See also related issues and PRs linked to from that issue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c5e3e7ebcdf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.