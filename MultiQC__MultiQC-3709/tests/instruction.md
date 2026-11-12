# Issue

**Title:** Fix wide parquet table merge losing samples or crashing

## Description

- [x] This comment contains a description of changes (with reason)

Fixes the merge of wide-format table data (`parquet_format: wide`) in `multiqc/core/plot_data_store.py`.

- The bug
  - Buffered wide tables were merged with a full join (`how="outer"`, renamed to `"full"` in #3708) on `sample` and `creation_date`, without coalescing the keys.
  - For any sample missing from earlier tables, every left-hand column came back null: `sample`, `creation_date`, `type`, `anchor` and so on. The duplicate `*_right` columns were then dropped, so those rows lost their sample name and `type`, and fell out of any `type == "table_row"` read.
  - With three or more tables it was worse: the second join produced `anchor_right` again and polars raised `DuplicateError: column with name 'anchor_right' already exists`, crashing the whole run. Reproduced on `main` with `MULTIQC_PARQUET_FORMAT=wide multiqc test-data/data/modules/{fastp,samtools,fastqc}`.
- The fix
  - Stack the existing and buffered tables with `pl.concat(how="diagonal_relaxed")`, then `group_by(["sample", "creation_date"])` and take the first non-null value per column.
  - Simpler than the join loop and the manual column bookkeeping it replaces, and it keeps first-seen sample order.
- Testing
  - New regression test in `tests/test_rerun.py`: two tables with partially overlapping samples; asserts all samples survive with non-null keys and correct per-table values. Fails on `main`, passes with the fix.
  - The same `fastp` + `samtools` + `fastqc` run now completes with 60 table rows, no null `sample`, `type` or `anchor`, and each table's metrics on the expected samples.
  - `tests/test_rerun.py` passes on polars 1.44 and 2.0.0; `mypy multiqc tests` is clean on both.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6cad06799a6e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.