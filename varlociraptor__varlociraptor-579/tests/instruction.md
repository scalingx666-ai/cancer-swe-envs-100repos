# Issue

**Title:** chore(master): release 8.11.0

## Description

:robot: I have created a release *beep* *boop*
---

## [8.11.0](https://github.com/varlociraptor/varlociraptor/compare/v8.10.0...v8.11.0) (2026-10-16)

### Features

* Add testcase from pm4onco seqc2 ffpe  benchmark for 1:154466505 () ()
* Add testcase from pm4onco seqc2 ffpe  benchmark for 19:54638714 () ()
* Add testcase from pm4onco seqc2 ffpe  benchmark for 8:11324507 () ()
* report fragment end bias in output BCF; obtain major fragment end independently for left and right side of fragments; add testcase from pm4onco seqc2 ffpe benchmark for 3:195784926 () ()
* support for optional fragment end bias detection (identifies likely FFPE artifacts) () ()

### Bug Fixes

* fixed expected error rate calculation; added ability to test all records in a testcase; confirmed that atomized variants are correctly not called as somatic in testcase test_seqc2_ffpe_1_3900324 () ()

---
This PR was generated with [Release Please](https://github.com/googleapis/release-please). See [documentation](https://github.com/googleapis/release-please#release-please).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4348b1fe807b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.