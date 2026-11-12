# Issue

**Title:** Verify ref ranges parquet ingest by reading number  of parquet rows using bq [VS-2032]

## Description

# Close the ref_ranges duplication gap in Parquet ingest verification

## Summary

On the Parquet ingest path, nothing detected a sample whose `ref_ranges` rows had been loaded twice. The duplicated reference blocks reached the callset, and the source Parquet was deleted as if the load were clean. `vet` had only a heuristic detector: a row count above 1.6 × the run median.

This PR replaces that heuristic with an exact check that covers every family. For each `(table, sample_id)`, `VerifyParquetLoading` compares the number of rows declared in the sample's Parquet file footers with the number of rows BigQuery holds. Deferred from VS-1989 (PR #9422).

## The check

- **Footer row counts.** A BigQuery external table over `<parquet_output_gcs_dir>/<family>/*.parquet` returns `COUNT(*)` grouped by `_FILE_NAME`. It runs as one query per family, for `vet`, `ref_ranges` and `sample_chromosome_ploidy`. BigQuery answers from footer metadata, so the query bills 0 bytes and needs no Parquet reader in any image.
- **Comparison.** Each sample's footer total is compared with BigQuery: `INFORMATION_SCHEMA.PARTITIONS.total_rows` for `vet` and `ref_ranges`, and a per-sample `COUNT(*)` for ploidy. More rows in BigQuery means a duplicated load, and fewer means a lost one. No threshold is involved.
- **One file per sample per family.** Ingest writes exactly one Parquet file per family per sample, so a pair with more than one listed file is flagged even when the sums agree. Without this rule, a sample loaded once from each of two files would read 2N against 2N and pass. That happens on a re-run against the same Parquet directory at a different scatter width, because the file name records the sample's position in its shard.
- **Unreadable files.** A listed file that the query returns no row for cannot be counted, so its sample is flagged as well.
- **URI scoping.** Each URI is scoped to one family directory and ends in `*.parquet`. That excludes `quarantine/`, sibling callsets, and the verification diagnostics.

## What a finding does

- The flagged sample's Parquet, in every family, is moved to `quarantine/` instead of deleted. The rest of the callset is deleted as normal.
- By default the run then aborts (`parquet_fail_on_quarantine = true`). The abort is a notification: the data is loaded, and the Parquet is kept.
- A footer finding cannot be waived. `parquet_allow_flagged_vet_loads` now waives only the vet truncation screen. Only `parquet_fail_on_quarantine = false` lets a run with a footer finding finish green.

## Scope limit

The check is exact with respect to loading, not generation. A footer's row count is the file's own report of its contents, so a file written upstream with doubled rows shows 2N in the footer and 2N in BigQuery, and the check reads that as agreement. The vet truncation screen, which flags samples at or below median / 1.6, remains the only detector for the low-side version of that case. This limit is documented in `verify_structural_checks.py` and `parquet_loading/README.md`.

## Interface changes

- **Removed:** the vet duplication screen and its input, `parquet_vet_duplication_threshold`. It was removed from `GvsImportGenomes`, `GvsBulkIngestGenomes`, `GvsJointVariantCalling` and both quickstart workflows.
- **Renamed outputs:**
  - `parquet_vet_duplication_flagged` → `parquet_row_count_mismatch_flagged`
  - `parquet_quarantined_duplication_samples` → `parquet_quarantined_row_count_mismatch_samples`

## Verification

| Run                                               | Samples | Result                                                                         |
|---------------------------------------------------|---------|--------------------------------------------------------------------------------|
| Quickstart integration `e520e6d3`                 | 3 × 5   | Succeeded. All five Parquet ingests agree for every pair; nothing quarantined. |
| Bulk ingest `bef389b6` (GVS Integration - Nalini) | 25      | Footer counts agree for all 25 pairs per family.                               |
| Bulk ingest `f5ceba4c` (NHGRI_AnVIL_3K_nalini)    | 1000    | Footer counts agree for all 1000 pairs per family; nothing quarantined.        |

On the 1000-sample run (3000 files):

- The three footer queries billed 0 bytes.
- They used 133,292, 281,036 and 91,629 slot-ms for vet, ref_ranges and ploidy, about 169 slot-ms per file.
- They finished in 577, 850 and 540 ms elapsed.
- `total_rows`, read straight after the load, agreed for all 3000 pairs.

On the 25-sample run, the vet truncation screen quarantined 6 samples. These are the table's 3 exome and 3 BGE samples, each with about 4% of the WGS median, and are not truncated loads. The footer check flagged none of them.

## Measured at 5k samples on AoU data

AoU `GvsJointVariantCalling` submission `5afeb3e2` (5,000 Foxtrot WGS samples, `aou-genomics-curation-prod`) succeeded end to end with nothing quarantined. Footer and BigQuery row counts agree for all 15,000 `(table, sample_id)` pairs, and the truncation screen flagged none (vet median 4,976,414.5 rows).

| Family     | slot-ms   | slot-ms/file | Elapsed | Bytes billed |
|------------|-----------|--------------|---------|--------------|
| vet        | 1,276,158 | 255          | 3.3 s   | 0            |
| ref_ranges | 1,947,636 | 390          | 1.9 s   | 0            |
| ploidy     | 2,125,211 | 425          | 2.2 s   | 0            |

- `total_rows` was read 2m14s after the last load task reported success and agreed for all 10,000 vet and ref_ranges partitions. That gap is the verification task's VM startup, so production runs have the same structure.
- All of `verify_all_loaded.py` took 19 s.
- Slot use per file roughly doubled from the 1k run (357 against 169 on average), so a linear extrapolation to about 500k samples, about 150 slot-hours, is a lower bound. The jobs ran on demand, so they billed nothing. Elapsed time grew 3 to 4 times for 5 times the files, which suggests minutes per family at 500k.

## Not yet measured at full AoU scale

Elapsed time and `total_rows` freshness at about 500k samples

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `01b13c21f0f1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.