# Issue

**Title:** SKCM High Weight Fix

## Description

As noted by @tjiagoM in #144 I was not extracting high weight genes based on standard deviation of the mean. Thanks for letting me know! :smile_cat: I was selecting extreme gene values from a different threshold. The top ranked genes were still selected, but several others were not.

This PR 

I will need to file a followup pull request running the two updated files (`results/high_weight_genes_node53_skcm.tsv` and `results/high_weight_genes_node66_skcm.tsv`) through the pathway interpretation analysis (`scripts/encoding_pathway_analysis.R`).

The package I use for ORA (`WebGestaltR`) was recently updated (`0.3.0`) and updates are not straightforward (see greenelab/tad_pathways_pipeline#31)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1b4ad8f8032c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.