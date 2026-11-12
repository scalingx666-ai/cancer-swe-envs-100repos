# Issue

**Title:** cyst region in 205 was an erroneous duplicate of a tumor

## Description

# Removed Duplicate Instance

## Description

cyst region in 205 was an erroneous duplicate of a tumor

## Checklist

- [x] Merged latest `master`
- [x] Updated version number in `README.md`
- [x] Added changes to `changelog.md`
- [x] Updated version number in `setup.py`
- [x] (only when updating dataset) Ran `annotation.import` to completion
- [ ] (only when updating dataset) Updated Surface Dice tolerances in `labels.py` (execute `sample_segmentations.py`, and 
  after that `compute_tolerances.py`. Then put the new values in. Do not re-use sampled segmentations from prior dataset versions!)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21698f97458b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.