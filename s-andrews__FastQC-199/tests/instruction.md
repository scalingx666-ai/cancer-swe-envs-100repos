# Issue

**Title:** Performance: Eliminate per-sequence `char[]` allocations in seven QC modules

## Description

`String.toCharArray()` allocates a fresh `char[]` the same length as the string, ~300 bytes per call. Seven modules each did this per sequence, so a single 50 M-read file produced ~100 GB of char arrays. The GC handled it, but at the cost of heap headroom and GC time. 

This branch replaces every such loop with `String + length + charAt`, which on JDK 17 intrinsifies to the same machine code as `char[]` indexing, minus the allocation and copy. Where the inner loop reads the same base more than once, the new code caches it into a local `char b` so the bounds check fires once per iteration. `PerSequenceGCContent.truncateSequence` switches from returning a `char[]` to returning a `String` for the same reason. 

Affects: BasicStats, NContent, PerBaseQualityScores, PerBaseSequenceContent, PerSequenceGCContent, PerSequenceQualityScores, PerTileQualityScores.

Benchmark report shows a small speed increase and a drop in peak RSS memory usage for single files. When running with multiple files however the memory usage is significantly less (~30% less). All `fastqc_data.txt`, `summary.txt`, and `fastqc_report.html` files remain byte-identical to master. Full report: [report.html](https://github.com/user-attachments/files/28120775/report.html)

<details><summary>Screenshot</summary>

<img alt="_Volumes_T7%20Shield_fastqc-bench_tochararray-full_report html" src="https://github.com/user-attachments/assets/6bb44c57-393f-4b1c-a364-c3fec4d7701a" />

</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b6bc4b908b6c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.