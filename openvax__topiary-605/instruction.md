# Issue

**Title:** Separate antigen source and exclusion species from MHC selection

## Description

Topiary's built-in sequence and exclusion sources defaulted to human even when variant annotations were mouse. Make source and background selection explicit while preserving independent MHC alleles: mouse antigens with human HLA and human antigens with a mouse comparison background are both valid.

- Add `--species` for antigen sources and independent `--exclude-species` / `--exclude-ensembl-release` settings. Default direct inputs remain human; non-human built-ins require an explicit release.
- Default exclusions inherit the actual single loaded annotation, including MAF/JSON references. Explicit source options must agree with those annotations. Ambiguous/custom references require an explicit background for built-in exclusions. Source coordinates are never reinterpreted.
- Check human-only CTA/tissue membership in the context it reads. Human CTA plus mouse background is supported. Presenting alleles are never inferred from either species setting.
- Preserve all exclusion sequences when independent reference sources reuse FASTA headers.

 and #604. Bump to **5.112.0**. Existing callers that deliberately used source flags to select a different exclusion background should use the new exclusion-specific flags instead.

Validation:
- 76 focused checks pass, including a composed CLI-to-CSV workflow for default human, mouse, HLA-transgenic mouse and human-xenograft configurations, plus paired source/protein-change reference resolution and explicit conflict handling.
- Deliberately forcing human source selection, ignoring the background override, or restoring the old FASTA overwrite each makes the relevant regression test fail at its output assertions.
- Real installed Ensembl 102 + MHCflurry weights: mouse Trp53 produces 382 peptides / 1,146 rows against HLA-A*02:01; original mouse coordinates and finite positive affinities verified.
- Real human default: TP53 produces 403 peptides / 1,209 rows. Exact exclusion against the 68,381-protein mouse reference retains 321 peptides; independent direct substring searches verify every inclusion/exclusion, and cached native scores and HLA identity are unchanged.
- Lint, strict docs, dependency checks and source/installed release metadata pass. The full local suite passes: 6,376 tests, 94% coverage. All 18 PR test jobs and the docs workflow pass; publication follows green merged-master CI.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2787a3fa2d41`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.