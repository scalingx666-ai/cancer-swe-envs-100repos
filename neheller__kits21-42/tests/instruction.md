# Issue

**Title:** added some code to simply wrap existing implementations in conversion…

## Description

# Handle MHA Inputs By Converting to Nifti

## Description

Change approach for official submissions accepting mha files to simply convert those to .nii.gz at an intermediate location and the run predictions as normal, and then finally converting the .nii.gz predictions back to the expected format.

## Checklist

- [x] Merged latest `master`
- [ ] Updated version number in `README.md`
- [ ] Added changes to `changelog.md`
- [ ] Updated version number in `setup.py`
- [ ] (only when updating dataset) Ran `annotation.import` to completion
- [ ] (only when updating dataset) Updated Surface Dice tolerances in `labels.py` (execute `sample_segmentations.py`, and 
  after that `compute_tolerances.py`. Then put the new values in. Do not re-use sampled segmentations from prior dataset versions!)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b7c1155daad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.