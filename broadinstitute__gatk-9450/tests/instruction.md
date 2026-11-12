# Issue

**Title:** Update Clinvar to use manually generated updates [VS-2029]

## Description

## VS-2029: Optionally annotate the VAT with a manually rebuilt ClinVar

The VAT's ClinVar annotations come from the ClinVar database bundled with Nirvana 3.18.1, which dates from October 2023 (`ClinVar_20231028`). Nirvana is no longer updated, so that ClinVar will not get any newer by itself. This PR adds an opt-in `use_manual_clinvar_update` input to `GvsCreateVATfromVDS`. When it is set, Nirvana annotates with `ClinVar_2025-07.nsa` instead. Aaron built that database from the July 2025 ClinVar XML using a patched SAUtils (VS-1994). It defaults to `false`, so existing runs are unchanged.

### What changed

- **`GvsCreateVATfromVDS.wdl`**
  - New inputs:
    - `use_manual_clinvar_update`, default `false`.
    - `manual_clinvar_path_prefix`, default `gs://gvs_quickstart_storage/Nirvana/ClinVar/ClinVar_2025-07`. The next ClinVar refresh needs only a new prefix.
  - The three ClinVar files (`.nsa`, `.nsa.idx`, `.nsa.schema`) are declared inside `if (use_manual_clinvar_update)`. When the flag is off they reach `AnnotateVCF` undefined and nothing is localized.
  - `AnnotateVCF` builds a writable directory of symlinks:
    - It links every bundled supplementary annotation file except `ClinVar_*.nsa*`, then links in the rebuilt files.
    - It then points Nirvana's `--sd` at that directory.
    - The approach works the same way with and without reference disks.
    - A guard fails the task early unless exactly one ClinVar `.nsa` remains.
- **`GvsQuickstartVATIntegration.wdl`** passes `use_manual_clinvar_update` through to `GvsCreateVATfromVDS`, so the flag can be set from `GvsQuickstartIntegration` in Terra. With the flag on, it compares against a separate set of expected outputs, in `vat_manual_clinvar/` beside the existing `vat/` (see "Truth data" below).
- **Docs**, in `scripts/variantstore/docs/vat_manual_ClinVar_updating/`

### Testing

| Submission (`gvs-dev/GVS Integration mcovarr`) | Flag | `use_reference_disk` | Result                                                                                                    |
|------------------------------------------------|------|----------------------|-----------------------------------------------------------------------------------------------------------|
| `6ee83e18-2181-4e54-ba0b-22b9c47d0da0`         | off  | `true`               | Passed. The VAT TSV is byte-for-byte identical to the baseline VAT's.                                     |
| `af0f9e5d-dd99-499f-b8ae-77ba3ae260f0`         | on   | `true`               | Differs from the flag-off run only in ClinVar columns, and reproduces every number in `validation.md` §3. |
| `c8d31878-9245-43e8-986c-1e3c47f0c7e9`         | on   | `false`              | Ran the download path fresh. The VAT TSV is byte-for-byte identical to the VAT Aaron QC'd in VS-1994.     |
| `cce970e2-f377-49be-bbf8-fc396b213fb8`         | on   | `true`               | Genome-wide. Differs from the existing `all_chrs/vat/` truth only in ClinVar columns.                     |
| `7da54224-f2fa-4b37-b35a-b49bd3d110bb`         | on   | `true`               | Passed against the new `vat_manual_clinvar/` expected outputs, call caching off.                          |

Notes on the runs:

- In every run, all 33 `AnnotateVCF` shards ran fresh, with no call-cache hits.
- `af0f9e5d`, `c8d31878` and `cce970e2` failed the quickstart's exact-match assertions because they were compared against the flag-off truth data. The change to `GvsQuickstartVATIntegration.wdl` gives flag-on runs their own expected outputs, and `7da54224` passes against them.
- `validation.md` §9 has the details, including the paths of the surviving VAT TSVs.

### For reviewers to decide

**The original ClinVar files.** The rebuilt ClinVar files were originally staged in `gs://gvs-internal/nirvana-manual-updates/`. This PR reads copies in `gs://gvs_quickstart_storage/Nirvana/ClinVar/`, a bucket readable by all authenticated users that already holds the VAT's MANE and Entrez inputs. The copies' checksums match the originals. Nothing references the `gvs-internal` originals any more. Can these be deleted now?

### Truth data

The flag stays off by default everywhere. We plan to build a one-off VAT for the VIA project with the rebuilt ClinVar, but not to use it for Foxtrot 2.1 / p5. Our callsets also deliver ClinVar VCF and PGEN files, and updating ClinVar for a real callset delivery should mean regenerating the ClinVar interval lists and remaking those deliverables, which is out of scope for 2.1 / p5. Golf may revisit that.

So the test needs to support both configurations, with its expected outputs chosen by the flag:

- **Flag off (default):** unchanged, `vat/` and `all_chrs/vat/` under `gs://gvs-internal-quickstart/integration/2026-07-22/`. CI keeps testing this path.
- **Flag on:** new `vat_manual_clinvar/` and `all_chrs/vat_manual_clinvar/` under the same prefix, each holding `vat_complete.bgz.tsv.gz` and `table_sizes.csv`, taken from `af0f9e5d` (chr20/X/Y) and `cce970e2` (genome-wide). Nothing existing under the prefix changes, so there's no new dated prefix and no bump of `GvsQuickstartIntegration.wdl:48`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `018e5930226f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.