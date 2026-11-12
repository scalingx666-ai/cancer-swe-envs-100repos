# Issue

**Title:** fixing rstrip and classification order for option 17

## Description

sort_nomal_txt function is updated (to accommodate the changes discussed for option 17). Added one extra print for classes and the has fixed the rstirp issue (Note that I have fixed rstrip issue only for the lines that were part of Option 17, there are other rstrip s that probably need to be fixed for other cases). Finally some minor formatting changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dcd8bbb8d97a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.