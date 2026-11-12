# Issue

**Title:** Do not index the 1-mismatch search's BW ranges with an N

## Description

`SeedAligner::oneMmSearch` looks for end-to-end hits with at most one mismatch, before seed search, by matching one half of the read exactly and then extending into the other half. In that second half, when the Burrows-Wheeler range so far covers more than one row, it read `t[rdc]`, `b[rdc]`, `tp[rdc]` and `bp[rdc]` before checking the read character. For an N (`rdc == 4`) that reads one element past the end of four-element arrays on the stack. ASan and UBSan report it whenever a read with one N falls in a repeat and `--n-ceil` allows the N, which it does by default.

**Change:** the lookup now happens only for A, C, G and T (4 lines in `aligner_seed.cpp`).

**Output does not change.** For an N, `match` is false, so the values read past the end were never used: the loop stops after trying the mismatch at that position, and the next pass reinitializes them.

**Test:** `test_one_n_in_repeated_read` in `scripts/test/test_regressions.py` aligns reads with one N to a reference made of two copies of the lambda genome. The repeat matters, because the bug needs the read's half to match more than one place. The test checks every alignment and, when `bowtie2-align-s-sanitized` has been built (as `make allall` does), runs it on the same input and requires a clean run.

**Verification:**
- **The test:** it fails on master's sanitizer build (`runtime error: index 4 out of bounds for type 'TIndexOffU[4]'` at `aligner_seed.cpp:1220`) and passes with the fix.
- **Output identity:** SAM output, ignoring `@PG`, is byte-identical to master on 5,000 random lambda reads with one N each (end-to-end and `--local`, against lambda and a doubled lambda genome), on paired lambda reads with `-k 50` against the doubled genome, and on 200,000 E. coli reads.
- **Builds and tests:** `make allall` succeeds, and the pytest suite gives 615 passed and 4 skipped on macOS arm64.

This was found by differential fuzzing with sanitizer builds, while reviewing the DP changes split out of #536.

This was created by Ben Langmead with help from Claude Opus 5.5

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `976e22118540`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.