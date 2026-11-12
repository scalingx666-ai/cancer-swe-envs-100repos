# Issue

**Title:** Fix a problem triggered by cram 3.1 support

## Description

Fixes a commented issue in #453: CRAM 3.1 input failed inside Clair3 even though samtools read the same
file fine.

The reporter noticed that the Docker image ships samtools/htslib 1.24 while Clair3's
own native library is built against the htslib downloaded by `build.py`, pinned at
1.15.1. Files written as CRAM 3.1 therefore failed with

    [E::cram_next_slice] Failure to decode slice
    [E::cram_decode_seq] CRAM CIGAR extends beyond slice reference extents

Fixes the default (`call_variants` C) path. Build the native library against htslib 1.24 instead of 1.15.1. And pass the reference to samtools mpileup so CRAM input can be decoded.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb625c7697df`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.