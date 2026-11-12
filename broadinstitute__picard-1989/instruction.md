# Issue

**Title:** Optionally extract all fingerprint snps

## Description

### Description

Currently ExtractFingerprints only writes out the representative SNP for each haplotype block. While this is useful for the intended use-case (of creating a fingerprint and then using it subsequently in crosscheck or somesuch) there are other use-cases where it can be useful to know the genotype/PL at _all_ the fingerprinting SNPs in each block. 

Currently, I've been developing a way to increase the power of VBID2 on low-coverage bams using the haplotype blocks. 

### Checklist (never delete this)

Never delete this, it is our record that procedure was followed. If you find that for whatever reason one of the checklist points doesn't apply to your PR, you can leave it unchecked but please add an explanation below.

#### Content
- [ ] Added or modified tests to cover changes and any new functionality
- [ ] Edited the README / documentation (if applicable)
- [ ] All tests passing on github actions

#### Review
- [ ] Final thumbs-up from reviewer
- [ ] Rebase, squash and reword as applicable

For more detailed guidelines, see https://github.com/broadinstitute/picard/wiki/Guidelines-for-pull-requests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b17c4725f15`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.