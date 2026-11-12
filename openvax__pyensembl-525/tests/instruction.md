# Issue

**Title:** Match nothing when a lookup's feature table or column is absent (2.33.2)

## Description

Stacked on #524 (2.33.1); merge that first. This branch contains its commit, so until then the diff here shows both.

## What was broken

The annotation index creates a table only for the features a GTF contains, and a column only for the attributes it uses. Lookups filtered on them anyway:

- #499: on a GTF with only gene/transcript/exon rows, `genome.transcript_by_id("t").complete` (and `contains_start_codon`, `start_codon_complete`) raised `sqlite3.OperationalError: no such table: start_codon`. The query was already marked optional (`required=False`), so the transcript should just report that it has no start codon.
- #502: on a GTF without `transcript_name` (e.g. the T2T-CHM13 rapid 2022_07 annotation), `genome.transcripts_by_name("CFTR")` raised `no such column: transcript_name`. So did `transcript_ids_of_transcript_name`, `exon_ids_of_transcript_name` and `gene_name_of_transcript_name`.

Both reproduce on main with the synthetic GTF from #499 (with and without `transcript_name`).

## Cause

`Database.query` built `SELECT ... FROM <feature> WHERE <filter_column> = ?` without checking that the table or the filter column exists.

## Fix

If the feature table has no filter column, `Database.query` returns no rows. An absent table has no columns, so the same check covers it. It reuses the existing `column_exists` (one cached `PRAGMA table_info` per table). The versioned-ID retry is skipped in that case too, since its second query would hit the same missing table. Callers then behave as they do for any value that isn't in the annotation:

- Optional transcript features: `contains_start_codon`, `contains_stop_codon`, `start_codon_complete`, `stop_codon_complete` and `complete` are `False`.
- Required features still raise. `start_codon_positions`, `stop_codon_positions` and `coding_sequence_position_ranges` raise `ValueError("Transcript t does not contain feature …")` instead of `OperationalError`.
- `transcripts_by_name` raises the same `ValueError("No results found …")` as for an absent name in an annotation that has names. Gene names are not used as transcript names (`transcripts_by_name("G")` still fails when `G` is only a gene name).

Existing indexes are used as they are; nothing is rebuilt.

This does not change accessors that *list* an absent attribute or table, e.g. `transcript_names()`, `transcript_names_of_gene_name()` or `protein_ids()` without a CDS table. Those still raise `OperationalError`, because what they should return (empty, `None`s, or an error) is a separate decision.

## Verification

- New `tests/test_absent_features_and_attributes.py` builds small GTFs in `tmp_path`. It covers absent start/stop/CDS tables (optional lookups give `False`; required accessors raise `ValueError`), lookups by transcript name and by gene name without `transcript_name`, and a control with names. 5 of its 6 tests fail on main with `OperationalError`; all pass here.
- `./lint.sh` clean, `./test.sh`: 1143 passed, and `./docs.sh` (strict) passes. `scripts/check_sdist.py` on the built sdist: 339 passed. `scripts/check_docs_examples.py`: 17 examples OK.

Version 2.33.2, with release notes.

https://claude.ai/code/session_01JHJhwj29oDqoeu7TYd34Pi

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3219771b395a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.