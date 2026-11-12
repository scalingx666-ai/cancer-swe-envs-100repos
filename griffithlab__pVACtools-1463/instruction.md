# Issue

**Title:** Fix pVACview aggregate report header tooltips

## Description

After the changes made in v7.x to the aggregated report, we never updates the header tooltips when displaying this file in pVACview. This PR fixes that. It also fixes a few existing tooltips to better describe the contents of the column.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f9762a92a69`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.