# Issue

**Title:** Give zip downloads their version (1.6.1)

## Description

Makes a zip or tarball downloaded from GitHub know its SigProfilerExtractor version. Without it the Workbench shows **v0**. This is the last change for 1.6.1.

- **Cause:** SPE's version comes from git tags (setuptools_scm). A GitHub download has no git history, so the version fell back to `0+unknown`, shown as "v0".
- **Fix:** `.git_archival.txt`, marked `export-subst` in `.gitattributes`. GitHub fills it in with the commit and nearest tag when it builds an archive, and setuptools_scm reads the version from it when there is no `.git`. With `git archive` of this branch, the version came out as `1.6.1.dev5+g868d105`; the same archive without the file gave `0+unknown`. A download of the `v1.6.1` tag will show "v1.6.1".
- **Fallback:** if the version still cannot be found (a folder copied without either), the top bar shows nothing instead of "v0".
- **Tests:** browser tests for the top-bar label (released, development and unknown versions), and a check on the archival files. Locally, 223 Workbench tests and SPE's unittest suite pass.
- **Changelog:** a line in the 1.6.1 section.

Once this is merged and CI passes, the merge commit will be tagged `v1.6.1`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `674fe3b28bb4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.