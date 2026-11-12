# Issue

**Title:** raise on vcf_parse failure instead of yielding a bad record

## Description

## Problem
`vcf_parse()` returns 0 on success and -1/-2 on failure. It never returns a positive value, so the three `ret > 0` checks never fire and the parse failure checks are silently ignored.

In `Writer.variant_from_string` the consequence is a segfault reachable with the following code:

```python
w = Writer.from_string(path, header)
v = w.variant_from_string("chr1\tNOTANUMBER\t.\tA\tC\t40\tPASS\t.\tGT\t0/0")
v.CHROM   # 'chr1'  -- no exception was raised
v.REF     # Segmentation fault (core dumped)
```

`vcf_parse` logs `[E::vcf_parse] Could not parse the position 'NOTANUMBER'` and bails out via its `err` label. The record comes from `bcf_init()`, zeroed from calloc. `vcf_parse` aborts at POS before `shared` (with ID/REF/ALT) is populated. `newVariant` calls `bcf_unpack`, which returns immediately with `if ( !b->shared.l ) return 0;` and leaves `d.allele` and `d.id` as NULL. Reading .REF or .ID de-references them, leading to segfault. 

## Fix
Test `ret != 0` instead of `ret > 0` at all three `vcf_parse` call sites. This matches the convention already used everywhere else in the file (e.g. lines 364, 448, 2172).

Also fixes the exception message in `variant_from_string`, which concatenated a `str` with an `int` and so would have raised `TypeError` rather than the intended message had it ever been reachable.

## Note on scope
The two region-iteration call sites have the same defect but are harder to exercise in a test, since it requires a bgzip+tabix-indexed file whose index builds while a line still fails to parse. They are fixed here because it is the same one-line defect. The added test covers the `variant_from_string` path only.

This does change behavior for malformed input in indexed files: previously a partially filled record was yielded silently, now an exception is raised.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8f22b53d3c9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.