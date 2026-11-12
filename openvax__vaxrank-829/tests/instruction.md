# Issue

**Title:** Expose named antigen type limits and clarify selection order

## Description

Replace `--min-antigens-per-type TYPE=COUNT` and `--max-antigens-per-type TYPE=COUNT` with explicit `--min-<type>-antigens COUNT` / `--max-<type>-antigens COUNT` flags for mutation, fusion, SV, CTA, ERV, splice, viral and isoform. For example, use `--min-mutation-antigens 5 --max-cta-antigens 8`. Each CLI override preserves the other configured type limits, including explicit zero values.

Help and documentation distinguish vaccine-wide unique-sequence counts from per-source window limits. They explain how both selection modes serve minimum goals and enforce maximum caps, then use either balanced type representation or the existing ranking to fill remaining capacity. A worked example shows different category counts under identical bounds. The API/YAML settings, scoring and allocation algorithm remain unchanged.

Validation: `./lint.sh` and 59 focused CLI, allocation and error-handling tests pass, including the mixed-antigen peptide/mRNA CLI, report and native-replay smoke. The complete local `./test.sh` suite passes (3,253 tests). GitHub CI is green on Python 3.11–3.14, both supported Hitlist versions and both supported msgspec versions; documentation and release-version checks pass. Version: 3.53.1.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da399afc0ea1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.