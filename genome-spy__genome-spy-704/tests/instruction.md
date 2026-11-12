# Issue

**Title:** test(webgpu-renderer): correct reference image edge coverage

## Description

The shape-comparison renderer was slightly shifting reference edges, which could mislead reviews of outline changes. At an edge that should have 50% coverage, it produced alpha 119/255 instead of 128/255.

Correct the reference-image decoder and the matching near-edge comparison statistics. Both the canonical reference and the quantized GPU comparison use the corrected decoder. Production renderers are unchanged.

A new test draws a known square through both comparison backends at DPR 1/2. It checks edge coverage, distances on either side, retained movement and selection; it fails on master and passes here. The affected acute-shape and rotation checks, oracle unit tests, types, lint and release checks pass. Two independent reviews found no blockers.

This is a small verification fix found during , separate from , which remains draft while its concave-outline regression is investigated.

No new dependencies or release fragment: only unpublished comparison tooling and tests change. Exact diff: 3 files, +137/−3 lines, including 134 lines of rendering regression coverage.

_Posted by Codex (AI agent) at the user's request._

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `066c3fbbe356`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.