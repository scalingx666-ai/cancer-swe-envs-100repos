# Issue

**Title:** Three dynamic-programming speedups from #536, with tests and a fix-up guard

## Description

This PR takes three self-contained changes to the dynamic-programming (DP) code from @jmschrei's auto-optimize PR #536, one commit each, so they can be reviewed and reverted separately. Each commit contains only that item's hunks from #536. Every changed line in them comes from #536's diff, and nothing from its other items is included. Two more commits are new: one adds tests, and one makes item 1's fill skip its fix-up work when no column needs it.

| Commit | From #536 | Change |
|---|---|---|
| ae90533 | item 20 | `BtBranchTracer` clears its visited-cell sets when a backtrace starts (`initBt`) instead of for every reference window (`initRef`), since most windows are never backtraced with it (`aligner_bt.h`). |
| d25ae74 | item 18 | `RedundantAlns`, which rejects a new alignment that shares a DP cell with one already found for the read, stores each alignment as runs along diagonals instead of one hash-set entry per cell (`aligner_result.*`). |
| ef9abbb | (new) | `scripts/test/test_redundant.py`: tests for the redundant-alignment check; see below. |
| 26e6383 | item 1 | The end-to-end 8-bit fill (`alignNucleotidesEnd2EndSseU8`) replaces Farrar's lazy-F loop with a prefix scan (`aligner_swsse_ee_u8.cpp`). |
| 4841efb | (new) | The same fill skips the scan and its extra pass for a column when no carry can change it, and the query profile puts padding rows inside the gap barrier; see below. |

Terms:
- **Striped layout:** Farrar's SIMD layout. Read row `L*iter + j` sits in lane L of vector row j, so a DP column is `iter` vectors.
- **Lazy-F loop:** Farrar's fix-up for vertical-gap (F) values. The first pass over a column carries F down only within each lane; the loop carries it one lane further per pass and stops when nothing changes.
- **Prefix scan:** a fixed log2(lanes)-step max-plus scan across lanes that computes, in one go, what the lazy-F loop converges to. One more pass down the column then folds the carries into F, H and the next column's E.

**Item 1, details.**
- **Portable:** it uses only the `sse_*` wrappers, so the SSE2, AVX2 (`-v256`) and ARM (simde) builds all get it. It is the only fill change in #536 that ARM users get.
- **Left out:** the AVX2-only `if(iter <= 4)` dispatch and its kernels (items 5, 25 and 9b), which share the same function, are not in this PR. Only the scan setup (`vpass1..16`, `vhlast`) and the general column loop are.
- **Metrics change:** the DP fix-up count reported by `--met-file` now counts `iter` rows for each column that makes the extra pass, instead of lazy-F passes. SAM output does not change, but metrics output does.

**The guard (4841efb), details.**
- **The problem:** when F almost never changes, Farrar's lazy-F loop exits after one test per column, but item 1's scan always makes its extra pass, a load and store of F, H and E for every row. RSEM's settings include `--gbar 99999999`, which bars gaps everywhere. With them, item 1 alone was 5% to 13% slower than the commit before it.
- **The test:** before the scan, the fill now applies the lazy-F loop's own termination test. The first pass leaves F at 0 in each lane's first row, and a carry can enter a lane only through that row. So if no lane's first row gains from the carry of the lane above it, after the gap barrier, every F, H and next-column E value is already final, and the scan and the extra pass are skipped. This is the same exactness argument the lazy-F loop relies on.
- **Padding rows:** the striped layout pads a column to a whole number of vectors, and the query profile gave those padding rows no gap barrier. With 32 lanes (`-v256`), the padding spans several lanes and passed carries between them, so the test above almost never held. The profile now bars the padding rows. They come after the last row of the read, which is always inside the gap barrier, so no real cell depends on them.
- **Effect:** on the RSEM-style run, the PR goes from 1.11x to 1.19x faster than master to 1.29x to 1.49x (see the speed table). On WGS and RNA-seq to the genome, where gaps are allowed, the guard is within noise.

**Item 18, details.**
- **Exactness:** the runs are built by the same walk over the edits as the per-cell loop they replace, and two alignments overlap if and only if they have the same reference and strand and some pair of runs shares a row and a diagonal. So the answer is the same as the per-cell lookup's.
- **Cost profile:** `overlap()` now compares the candidate against each stored alignment's bounding box, then merges run lists only for the candidates that pass. That is linear in the number of stored alignments, instead of one hash lookup per cell. It was timed with `-k 200` (the RSEM-style rows below), where it gives most of the PR's gain.
- **Leftovers:** `RedundantCell` in `aligner_result.h` is no longer used, and `init(npos)` ignores `npos`. They are kept so the commit contains only #536's lines; they can be removed in a follow-up.

**Tests.** `test_redundant.py` adds 8 cases.
- **Copies:** a reference holds five copies of a segment: unchanged, with a 3-base deletion, with 20 extra bases, with two mismatches, and reverse complemented. With `-k 50` and `-a`, end-to-end and `--local`, each read must be reported exactly once per copy, with the expected gaps.
- **Lambda:** with `-a` on the lambda example, unpaired and paired, end-to-end and `--local`, no two alignments of a read may share a cell, and no two concordant pairs may share a cell in both mates.
- **Sensitivity:** the redundancy check matters only when separate DP windows reach the same cells, as with seed hits on either side of a long gap, or a mate that mate rescue finds again. The cases are chosen so that every one of them fails if `overlap()` always answers no, and the copies cases fail if it always answers yes. They pass on master and on each commit.

**Output:** unchanged. The release-build SAM was byte-identical to master at every commit in every case below, ignoring the `@PG` line.
- **Lambda:** end-to-end and `--local`, paired and unpaired, `-k 5`, `-a` (unpaired, paired, local paired), `--very-sensitive`, `--very-fast`, `-p 4 --reorder`, `--mm`, the long reads end-to-end and local, and simulated 250 bp reads.
- **Lambda, repetitive:** a doubled lambda genome (two identical chromosomes) with `-k 50` (unpaired, paired, long reads) and `-a` (unpaired, local paired).
- **E. coli:** 200,000 pairs end-to-end, `--local`, unpaired, `-k 10`, `-a`, `--very-sensitive`, at `-p 1` and `-p 8 --reorder`; and 20,000 simulated 250 bp reads end-to-end, local and `-a`.
- **Gaps barred or costly:** RSEM's settings on lambda and on E. coli, `--gbar 99999999` alone, `--gbar 10`, and `--rdg 8,4 --rfg 8,4`, at the branch head.
- **hg38:** HG002 WGS, GEUVADIS RNA-seq aligned to the genome, and RNA-seq aligned to GENCODE transcripts with RSEM's settings, at `-p 1` and `-p 16`, in both the SSE and `-v256` builds.

**Differential fuzzing.** A harness built random small genomes (with repeats, tandem repeats and Ns), reads of 1 to 320 bp with mismatches and indels, unpaired and paired, and random scoring options (`--rdg`, `--rfg`, `--gbar`, `--mp`, `--np`, `--score-min`, `--dpad`, `-k`/`-a`, seed options; about 80% end-to-end), and compared each run's SAM with master's. It flagged runs when the redundancy check was disabled or one step of the prefix scan was removed, so it is sensitive to the kind of bug these changes could introduce.
- **x86 SSE and `-v256` builds:** 12,300 runs each, about 65 million SAM records each, no differences. In each build, 8 runs hit the 2 GB memory cap the harness sets (with `-a` on repetitive genomes). Rerun without the cap, all 16 were identical to master, and the branch used less memory in every one of them (up to 7% less).
- **ARM (Apple M4 Pro):** 4,200 runs, about 44 million records, no differences.
- **Sanitizer build (ASan and UBSan):** 600 runs, identical to master, and no reports in code this PR changes.
- **Debug build:** 400 runs against master's debug build. Every run that aborted on a debug assertion aborted at the same assertion on master, and the others matched.

The debug build ran to completion on the long reads, the 250 bp reads and the paired lambda reads end-to-end, including with `-a`, with RSEM's settings and with `--gbar 99999999`, and its output matched master's release build. Its sampled cell checks cover the columns the guard skips. With `--local` on the long reads, the debug build stops on the debug-only assertion that #542 relaxes, exactly as master's does; with that fix applied, it completes and matches too.

**Tests:**
- **Builds:** each commit builds on its own, and `make allall` succeeds at the branch head.
- **Pytest suite:** 622 passed and 4 skipped on macOS arm64 at the branch head (the 614 of #541's suite plus the 8 new cases).

**Speed.** Intel Xeon Gold 6448Y, GRCh38 no-alt index (GENCODE transcripts for RSEM), 250,000 read pairs, median of 3 runs. The nodes were shared, so expect run-to-run noise of about 1% to 5%, and up to 10% for single-threaded WGS on this run. Speedups are master's time divided by each commit's, cumulative: "+18" includes item 20, "+1" is the three #536 items, and "+guard" is the whole PR. All four builds ran in the same job for each input.

| Input | Threads | Build | +18 | +1 | +guard |
|---|---|---|---|---|---|
| HG002 WGS 2x151 | `-p 1` | `-v256` | 1.07x | 1.53x | **1.50x** |
| | | SSE | 1.09x | 1.41x | **1.36x** |
| | `-p 16 --reorder` | `-v256` | 1.05x | 1.34x | **1.39x** |
| | | SSE | 1.08x | 1.29x | **1.28x** |
| RNA-seq 2x76 to hg38 | `-p 1` | `-v256` | 1.05x | 1.57x | **1.56x** |
| | | SSE | 1.06x | 1.53x | **1.52x** |
| | `-p 16 --reorder` | `-v256` | 1.04x | 1.35x | **1.34x** |
| | | SSE | 1.04x | 1.32x | **1.33x** |
| RNA-seq 2x76 to GENCODE transcripts, RSEM settings (`-k 200`, `--gbar 99999999`) | `-p 1` | `-v256` | 1.25x | 1.19x | **1.49x** |
| | | SSE | 1.28x | 1.11x | **1.32x** |
| | `-p 16 --reorder` | `-v256` | 1.23x | 1.18x | **1.42x** |
| | | SSE | 1.26x | 1.12x | **1.29x** |

In an earlier run that also timed item 20 by itself, it gave 0.99x to 1.07x, within noise, and the other columns agreed with this table to within a few percent. In the fastest WGS rep at `-p 1`, "+1" and "+guard" took 128.3 s and 128.4 s (SSE) and 116.8 s and 117.5 s (`-v256`), so the guard's apparent cost on WGS in the table is noise.

On an Apple M4 Pro (ARM, so the simde build) with E. coli (200,000 pairs, `-p 1`, median of 3), the three #536 items gave 1.64x end-to-end. With `--local` they gave 1.00x, as expected, since none of the three changes the local-mode fill. On 50,000 E. coli pairs, the guard was neutral with default options and made RSEM's settings about 20% faster than item 1 without it.

Item 1 carries most of the gain on genome alignment, in both builds. Item 18 is what helps the RSEM-style run, where `-k 200` makes many alignments per read, and the guard keeps item 1 from giving that gain back.

**Credit.** The three speedups are @jmschrei's work, found by his auto-optimize run in #536. He is the author of those three commits. This particular PR, drawing on elements of 536, was split out and prepared by Ben Langmead with help from Claude Opus 5.5.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `976e22118540`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.