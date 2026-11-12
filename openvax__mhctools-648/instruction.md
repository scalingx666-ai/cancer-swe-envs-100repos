# Issue

**Title:** Resolve allele identity migration plan review (4.8.2)

## Description

The #631 migration plan excluded supported `AlleleWithoutGene` names, treated `Pair` as sufficient validation, left class-II identities inconsistent with Topiary, and did not distinguish G/P groups from expression suffixes. This revision makes those policies explicit and adds concrete fixtures and downstream release gates.

- Preserve native BoLA-T2b/T2c identities; require same-species, ordered class-II alpha/beta chains for pairs.
- Keep canonical beta-only identities unpaired in both repositories, with explicit keyword migration and cache/output mapping rules. Preserve curated mouse haplotype pairs through the parser's documented collapse default.
- Reject G/P typings as single-allele identities using parsed annotations in addition to `is_allele_group`.
- Keep historical aliases disabled for generic identity; permit only verified native-catalog alias mappings, with provenance and explicit current-name requests.
- Require DR/DQ and non-human cross-repository fixtures, matching Topiary group validation, and migration rules for old inferred-pair caches.

References #631; it remains open for the future implementation. This PR changes the plan and required version string only, released as [4.8.2 on PyPI](https://pypi.org/project/mhctools/4.8.2/).

Validation: 14 canonical parser round trips plus BoLA/native-list, invalid-pair, group-chain, G/P and alias probes pass with mhcgnomes 3.68.1. Lint, documentation links (including the plan's relative source link), strict MkDocs and isolated-environment `pip check` pass. Both the pre-merge and clean-master deployment suites passed **2,344 tests with zero skips**; the 23 known BigMHC warnings are tracked in #615. All 18 GitHub CI/build checks passed before merge. Author review is recorded on this PR.

Deployment completed through `./deploy.sh`. The published wheel and source archive SHA256 hashes match the local build, and tag `v4.8.2` points to merge commit `c65743822c11739162705a264ca13d629fce3844`. The release lock was removed and master is clean.

The newly isolated optional-parser-mode defect is filed as . The chosen plan uses the documented default and needs no workaround for it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a3506272f9c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.