# Issue

**Title:** DEV-4465 Adjust timeout values for testing

## Description

The current 60 second timeout is being hit during the time of a multi-tarfile download, where the second tarfile contains 609 "large files", and doesn't start within 60 seconds of the end of a 1 file "Small files" tar.  

This fix sets it to none, to mimic the behavior before the latest changes to gdc-client. If value is chosen, setting it in conftest.py and putting it in the right place for this fix would be easy.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `36318a20bb94`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.