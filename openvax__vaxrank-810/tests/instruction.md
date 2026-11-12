# Issue

**Title:** Plan MS-dependent prediction scoring across antigen categories

## Description

Scale candidate binding/presentation predictions with MS evidence using `C = B ** (1 / G)`, where `G = 1 + ms_gain * log2(1 + M)` and M is deduplicated, weighted support. The default gain is 1 and there is no hard cap; an optional `max_relaxation` must be explicitly configured.

The [design](https://github.com/openvax/vaxrank/blob/docs/antigen-ms-evidence-plan/notes/design/antigen-ms-evidence.md) applies this algorithm across antigen categories and builds on #809. It defines allele provenance, biological recurrence versus publication corroboration, unchanged prediction-only scores when evidence is absent, and a single MS increment per peptide. Named soft prediction gates use the adjusted score while all other filters remain mandatory.

True zero predictions remain zero. Missing predictions remain explicit and require a configured MS-only scoring criterion plus direct experimental admission before automatic ranked selection. The plan also specifies prediction-independent candidate enumeration and separate serialized baseline/adjusted/increment score components.

This is a design-only draft. It includes ten worked examples, Hitlist/IEDB coverage reconciliation and implementation acceptance criteria. Bump the version to 3.51.6 as required for documentation PRs, based on current main 3.51.5; recheck before merging. #789 remains an implementation prerequisite, and #804 was fixed by #809.

Validation:
- `./lint.sh`, `git diff --check`, dependency compatibility and release-version validation passed.
- Ten examples and synthetic checks cover deduplication/reference grouping, optional caps, uncapped growth, zero gain, zero/one/missing predictions, log-space evaluation, soft-threshold crossing and a single MS increment.
- The formulas were evaluated through Topiary 5.111.3 in an isolated Python 3.11 environment, with Hitlist 2.1.9 and MHCtools 4.9.3.
- GitHub CI is green at `dae52ca`: full Python 3.11–3.14 jobs, Hitlist compatibility, supported-msgspec, docs and release-version checks.
- Local full `./test.sh`: 3,193 passed and three temporary-Git fixture setup errors (2,070.73 s). All seven cases selected by the affected test names passed on rerun without code changes; the three original Git commands also succeeded when retried in the retained fixtures. This is not a clean full local run. Missing subprocess diagnostics are tracked in #812; the original cause remains unconfirmed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2584701f6966`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.