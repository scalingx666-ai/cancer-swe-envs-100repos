# Issue

**Title:** Allow passing read name to mappy

## Description

This adds the (optional) ability to pass the read
name to the mappy `map` method.  Without the
read name, the call to `map` can sometimes give
different output than the command line version
of `minimap2` because of the way minimap uses
the hash of the read name to break ties in ordering hits.  
This can affect which / if certain supplementary alignments 
are generated, and even which / if non-primary alignments 
are generated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fcb5d5e6eb9f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.