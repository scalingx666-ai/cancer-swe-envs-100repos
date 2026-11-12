# Issue

**Title:** Only include paired-end sequencing reads in discordants file

## Description

I've encountered a few CRAMs in the wild where there are reads that aren't from paired-end sequencing runs. These did not have the BAM_FMUNMAP bit set so I've added a check that the sequencing run was paired to the section writing to the discordants file.

I also reindented to remove a bunch of tabs.

I've a test in process and the code change seems to help, but please review critically. cc @brentp @ryanlayer 

Thanks!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `178d7a6745d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.