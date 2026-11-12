# Issue

**Title:** Fix various issues related to database changes

## Description

I've added a temporary fix to get around the pfam legacy certificate expiration. I've also fixed the problems with pulling data from uniprot - both getting the uniprot ID from the gene name and extracting domain information from the API.
Given pfam will cease to exist soon I also changed the default behaviour so that the data is being pulled from uniprot by default, with pfam legacy being used when -pfam flag is added. Addresses #67 #45

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa1c5e35072f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.