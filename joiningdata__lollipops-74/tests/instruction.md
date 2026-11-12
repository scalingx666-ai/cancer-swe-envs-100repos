# Issue

**Title:** Switch to InterPro API

## Description

As you might be aware, the [Pfam legacy website](http://pfam-legacy.xfam.org) will be decommissioned very soon, which will cause `lollipops` to fail when using the `-pfam` option.

It also has been reported that domains and motif features are not displayed when retrieving data from UniProt (#73).

This PR changes the source of Pfam domains and motif features to the InterPro API (the `-pfam` and `-l` options were removed).
I've also added a call to the UniProt API to retrieve the protein length.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55938166cf37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.