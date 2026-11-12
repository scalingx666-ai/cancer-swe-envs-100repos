# Issue

**Title:** Use the PyUnicode API for str-returning functions instead of computing in bytes and decoding

## Description

Numerous fields construct strings that we know contain only ASCII, but the Python 2 provenance of the code means that we build them as `bytes` and then convert to strings. By using the PyUnicode_* API directly we can write the final `str` directly and avoid extra copies and decoding.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e06bd6355588`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.