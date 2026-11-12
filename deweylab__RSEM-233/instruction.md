# Issue

**Title:** Remove Boost

## Description

**Refactor:**
- Modernize to C++17 and remove boost dependency in core RSEM and pRSEM
- Boost-compatible RNG for reproducible results

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8bc1e2115493`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.