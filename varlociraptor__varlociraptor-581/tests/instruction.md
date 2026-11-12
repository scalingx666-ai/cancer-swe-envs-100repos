# Issue

**Title:** feat: support fragment end bias setting per sample in scenario

## Description

### Description

<!--Add a description of your PR here-->

### QC
<!-- Make sure that you can tick the boxes below. -->

* [ ] The PR contains a test case for the changes or the changes are already covered by an existing test case.
* [ ] The documentation at https://github.com/varlociraptor/varlociraptor.github.io is updated in a separate PR to reflect the changes or this is not necessary (e.g. if the change does neither modify the calling grammar nor the behavior or functionalities of Varlociraptor).
* [ ] Run benchmark (check this box to trigger the benchmark workflow)

<!-- This is an auto-generated comment: release notes by coderabbit.ai -->

## Summary by CodeRabbit

* **New Features**
  * Fragment-end bias can now be configured independently for each sample, allowing variant analysis to apply the setting selectively.
  * When the command-line option is enabled, fragment-end bias is applied to all samples; otherwise, each sample’s configured setting is used.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4c8b7b38998`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.