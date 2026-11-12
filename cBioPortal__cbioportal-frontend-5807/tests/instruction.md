# Issue

**Title:** Show normal allele freq and mutation status by default for CH mutations

## Description

## What

Mutations with a mutation status of `CH` (clonal hematopoiesis) are called in the matched normal (blood) sample, so their normal allele frequency is what matters. When the mutation table contains any CH mutation:

- **Patient view**: the **MS** and **Normal Allele Freq** columns are shown by default. The normal column (added in #5804) is still left out entirely when no mutation has normal read counts.
- **Results view**: the **MS** and **Allele Freq (N)** columns are shown by default. Before, both were hidden unless the user turned them on.

The MS column also shows CH as a **CH** badge with a tooltip, like the S/G badges, and the MS legend gets a "Clonal hematopoiesis" entry. Right now a CH status shows as small grey text.

Matching ignores case and accepts `CH`, `CHIP` or any value containing "clonal hematopoiesis". The MSK portal uses `CH`. No public study on www has CH mutations.

Default visibility for tables without CH mutations is unchanged. `skin_*_mutation_table_columns_show_on_init` still overrides the defaults when it is set.

## Preview

- https://deploy-preview-5807.preview.cbioportal.org/

There is no CH data on www. The screenshots will come from a portal that has CH mutations.

## Testing

- Unit tests for CH detection and the badge in `MutationStatusColumnFormatter.spec.tsx`
- Checked locally against www by rewriting `mutationStatus` to `CH` in the API responses: the patient view (lgg_ucsf_2014 P04) and the results view (TP53) show the MS and normal allele freq columns with CH badges, and without CH both are unchanged

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_01SFKQgm7Saqbzgd4i2zm2pz

<img width="1128" height="363" alt="image" src="https://github.com/user-attachments/assets/f8084815-467e-48ee-af12-53a78323c128" />


<!-- codesmith:footer -->
---
<a href="https://app.blacksmith.sh/cBioPortal/codesmith/cbioportal-frontend/pr/5807?autoLogin=true&ref=codesmith_pr_footer"><picture><source media="(prefers-color-scheme: dark)" srcset="https://pr-comments-assets.blacksmith.sh/codesmith/view-with-codesmith-dark-v3.svg"><source media="(prefers-color-scheme: light)" srcset="https://pr-comments-assets.blacksmith.sh/codesmith/view-with-codesmith-light-v3.svg"><img alt="View with [code]smith" src="https://pr-comments-assets.blacksmith.sh/codesmith/view-with-codesmith-dark-v3-3x.png" width="162" height="28"></picture></a> <a href="https://backend.blacksmith.sh/track/enable-autofix?expires=1795799624&installation_model_id=19627&pr_number=5807&ref=codesmith_pr_footer&repository=cBioPortal%2Fcbioportal-frontend&return_to=https%3A%2F%2Fgithub.com%2FcBioPortal%2Fcbioportal-frontend%2Fpull%2F5807&signature=b164694297f222140852837a5a5c4a8cc681319ab8bbc90225481731875b6171"><picture><source media="(prefers-color-scheme: dark)" srcset="https://pr-comments-assets.blacksmith.sh/codesmith/autofix-with-codesmith-dark.svg"><source media="(prefers-color-scheme: light)" srcset="https://pr-comments-assets.blacksmith.sh/codesmith/autofix-with-codesmith-light.svg"><img alt="Autofix with [code]smith" src="https://pr-comments-assets.blacksmith.sh/codesmith/autofix-with-codesmith-dark-3x.png" width="190" height="28"></picture></a>
<sup>Need help on this PR? Tag <code>@codesmith-bot</code> with what you need.</sup>

<!-- codesmith:autofix:disabled -->
<!-- /codesmith:footer -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f3d7d4d1c42`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.