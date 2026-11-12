# Issue

**Title:** fix non termination in interchromfilter

## Description

reads with unmapped mates can get into discordants
this fix makes sure that does not result in non-termination.

@ernfrid can you take a look?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6fda1010f3b3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.