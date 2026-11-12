# Issue

**Title:** Moves installations from root

## Description

+ conda and conda installs moved from root to fix issues with singularity
+ moves `smoove` install into `docker-build.sh`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `22973ffd77e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.