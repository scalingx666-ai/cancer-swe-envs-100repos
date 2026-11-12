# Issue

**Title:** Restore the variant-matrix output for full-alignment training (issue #484)

## Description

CreateTrainingTensorDirect.py arrived in b697c512 without the C side it expects: it passes a 14th argument (output_variant_matrix) and reads fa_data.allele_matrix / hp_labels / num_nearby_variants / has_variant_matrix, none of which existed on main. The full-alignment training path therefore died with "TypeError: calculate_clair3_full_alignment expected 13 arguments, got 14".

Bring the C implementation over from c6b2404 (cffi-subset), three-way merged onto main so that ff2e06d's BAM/CRAM read-error handling is preserved, and pass the new argument from the two calling-path callers and from fa_test.

Verified on a test image:
- CreateTrainingTensorDirect (FA path) writes tensors instead of raising
- --output_variant_matrix with --phased_vcf_fn produces hp_labels, phasing_matrix and phasing_num_variants
- variant calling is byte-identical to v2.0.5 on the ONT demo (553 records) and on the CRAM 3.1 chr22 region, with no decode errors

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ca47c1f3e004`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.