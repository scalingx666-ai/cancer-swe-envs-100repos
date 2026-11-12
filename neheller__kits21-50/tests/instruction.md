# Issue

**Title:** an alternative get imaging script

## Description

# Backend For Test set and Alternative Get Imaging

## Description

Merges code used for test set and adds a new get_imaging alternative that will hopefully work better on Windows

## Checklist

- [x] Merged latest `master`
- [ ] Updated version number in `README.md`
- [ ] Added changes to `changelog.md`
- [ ] Updated version number in `setup.py`
- [ ] (only when updating dataset) Ran `annotation.import` to completion
- [ ] (only when updating dataset) Updated Surface Dice tolerances in `labels.py` (execute `sample_segmentations.py`, and 
  after that `compute_tolerances.py`. Then put the new values in. Do not re-use sampled segmentations from prior dataset versions!)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7961ec1210d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.