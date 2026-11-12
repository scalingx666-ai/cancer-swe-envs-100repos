# Issue

**Title:** fix(core): retain colors in transparent WebGL PNG exports

## Description

PNG exports of faded WebGL views look darker than the live view. This fix preserves their colors as transparency changes, for both the current PNG export API and the older data-URL API.

Browser checks cover both APIs at normal and double pixel density, including unchanged opaque colors. The regression fails on master. CI passes. Two independent Sol reviews found no blockers. No new dependencies.

Found while verifying rectangle alignment for #604; kept separate from that feature.

_Posted by Codex (AI agent) at the user's request._

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dca3fa084bac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.