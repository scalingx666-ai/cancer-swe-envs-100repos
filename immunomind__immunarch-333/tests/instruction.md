# Issue

**Title:** Immunarch 0.9.0 release

## Description

- version bump
- Fix for phylip dnapars on Windows
- fix for path separators on Windows
- shell fix for Windows
- Added support for groups coloring on repClonalFamily() visualization
- repDiversity bugfixes
- Support for MiXCR 4.1 format and manual count column selection
- Tests for repAlignLineage; small fixes
- 10x_filt_contigs parser: fix for groups with same sequences in different order
- Tests for repClonalFamily()
- fix
- SHM tests
- WIP
- Implemented CDR and FR parsing from 10x_filt_contigs
- Added FR and CDR columns to AIRR format parser
- io_utility bugfixes
- Update for BCR pipeline tutorial
- CRAN fixes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc90fed75a32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.