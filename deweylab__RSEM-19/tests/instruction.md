# Issue

**Title:** Convert RSEM to use the HTSlib API instead of the legacy samtools API

## Description

This pull request converts the RSEM source code to use HTSlib for its BAM-reading needs rather than the old samtools 0.1.x API.  Since the old API is unmaintained (and has been for several years), we would recommend converting to the new API wholesale rather than continuing with the old API (cf @outpaddling's samtools/samtools#531).

This compiles against just htslib, but I have not tested it or even tried to run it.  So there would need to be a bedding-in period, especially of the fresh code in 5852e1ef73648606e02eb92c0bc49c0ccab121b4.

I realise you have some local changes in _samtools-1.3/bam_sort.c_ (ccec130769207255411170f316587b5160ff6ae0).  We are still considering how best to deal with your samtools/samtools#520; most likely mainline `samtools sort -n` will gain an ability to keep mates together.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f522f336133`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.