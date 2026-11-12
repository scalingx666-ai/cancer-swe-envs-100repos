# Issue

**Title:** Make the AVX2 build's output identical to the SSE build's, including `@PG`, and test the dispatch

## Description

On x86-64, `make` builds two aligners: `bowtie2-align-s` with SSE2 and 128-bit vectors, and `bowtie2-align-s-v256` with `-march=x86-64-v3` and 256-bit vectors. At startup the first checks the CPU and, if it qualifies, executes the second. Until #526 that check read AVX2 from the wrong CPUID leaf, so practically every x86 user ran the 128-bit build. With #526 merged, every x86-64-v3 CPU now runs the `-v256` build, and that build can give different output from the 128-bit one for some `--score-min` settings. This PR makes the two builds round floating-point arithmetic the same way, so their SAM records are identical, and makes the `-v256` build name itself as the program the user ran, so the `@PG` header line is identical too. It also adds tests of the dispatch and of that identity. It is item 2 of @jmschrei's #536, apart from the CPUID detection, which #526 covers.

| Commit | From | Change |
|---|---|---|
| b28c951 | #536 item 2 (@jmschrei) | The `-v256` build compiles with `-ffp-contract=off` (`Makefile`). |
| 626f02c | #536 item 2 (@jmschrei) | When executing the `-v256` binary, `check_x86_64_v3()` passes on the original `argv[0]` (`bowtie_main.cpp`). |
| 9338323 | (new) | `scripts/test/test_dispatch.py`: tests of the dispatch and of output identity between the two builds; see below. |

Terms:
- **x86-64-v3:** the x86-64 feature level that adds AVX, AVX2, BMI1, BMI2, F16C, FMA, LZCNT, MOVBE and XSAVE to the SSE4.2 level. Intel CPUs since Haswell (2013) and AMD CPUs since Excavator (2015) have it.
- **Dispatch:** `bowtie2-align-s` (and `-l`) executing `bowtie2-align-s-v256` (or `-l-v256`) in its own directory when the CPU qualifies.
- **Fused multiply-add (FMA):** one instruction that computes `a*b + c` with a single rounding, where a separate multiply and add round twice.
- **Floating-point contraction:** the compiler fusing `a*b + c` in source code into an FMA. GCC does it by default (`-ffp-contract=fast`) whenever FMA is available, which `-march=x86-64-v3` makes it.

**Commit b28c951 (#536 item 2), details.**
- **Why output could differ:** `SimpleFunc::f()` evaluates `--score-min`, `--n-ceil` and `-i` as `C + L*f(x)` for read length `x` and truncates the result to an integer. Compiled with contraction, the `-v256` build fuses that expression. When `C + L*x` lies within one rounding of an integer, the fused and unfused results truncate to different integers, so the minimum score differs by one, and with it which alignments are valid and their MAPQ.
- **Which settings are affected:** the built-in defaults are single-precision constants and, for every read length from 5 to 1,000, give the same integers either way. Values given on the command line are parsed as doubles, and some of them do hit such a length. For example, `--score-min L,-0.6,-0.6` (the default, typed out) hits it at 24 bp and at every fifth length from 109 to 209 bp; `L,-0.7,-0.022` hits it at 150 bp.
- **The fix:** `-ffp-contract=off` on the `-v256` build only. It changes no other build. The dynamic programming is integer SIMD, so the flag does not touch any vectorized loop.

**Commit 626f02c (#536 item 2), details.**
- **What changes for users:** bowtie2 prints `argv[0]` in the `CL:` field of the `@PG` header line and in the first line of `--version`. Since #526, on x86-64-v3 CPUs those name `bowtie2-align-s-v256` (or `bowtie2-align-l-v256`), and elsewhere `bowtie2-align-s`. With this commit they name `bowtie2-align-s` (or `-l`) on every CPU, as they did in releases before #526.
- **Why:** we think this makes somewhat more sense, since users expect all instruction-set specializations to behave the same. The `-v256` build is an implementation detail of how the same program runs on a given CPU, and its SAM records are identical to the 128-bit build's, so its header should be too. Output from one job then does not depend on which machine of a mixed cluster ran it.
- **What is still visible:** the `Options:` line of `--version` shows `-march=x86-64-v3` for the `-v256` build, so users and bug reports can still tell which build ran.

**Tests (9338323).** They run on x86-64 Linux when the `-v256` binary is built, and skip elsewhere (macOS, ARM, `SSE_AVX2=0`).
- **Dispatch:** the `Options:` line of `bowtie2-align-s --version` shows `-march=x86-64-v3` if and only if `/proc/cpuinfo` lists every x86-64-v3 feature, and its first line names `bowtie2-align-s` either way. Without a `-v256` binary beside it, `bowtie2-align-s` stays with the 128-bit code.
- **`@PG`:** the `@PG` line written by `bowtie2-align-s` names `bowtie2-align-s`, not the `-v256` binary.
- **Output identity:** each case runs the `-v256` binary and a copy of `bowtie2-align-s` placed where it cannot dispatch, and compares their SAM, apart from `@PG`. Five cases use the lambda example (unpaired, paired, local paired, long reads local, `-k 5`). Four use 2,000 reads of 17, 24 or 150 bp cut from lambda with random mismatches and qualities, at `--score-min` values on a rounding boundary for that length, in end-to-end and local mode.
- **Sensitivity:** built without `-ffp-contract=off`, all four boundary cases fail (for example, the same alignment of a 24 bp read gets MAPQ 3 from the `-v256` build and 0 from the 128-bit build). With it, all 12 tests pass.

**Output.** On an Intel Xeon Gold 6248R (GCC 13.1), the SAM output of `bowtie2-align-s-v256` and `bowtie2-align-s` was compared, ignoring `@PG`, for three builds: master before #526 (75c8565), master with #526, and this PR.
- **Lambda:** end-to-end and `--local`, unpaired and paired, `-k 5`, `-a`, `--very-sensitive`, the long reads end-to-end and local, and paired reads with `--score-min L,-0.7,-0.022 --trim-to 3:150`.
- **E. coli:** 200,000 simulated 150 bp pairs (`wgsim`) at `-p 8 --reorder`: paired end-to-end, `--local` and `-k 10`; unpaired end-to-end; and `--score-min L,-0.7,-0.022`, unpaired and paired.
- **With this PR:** all 16 cases byte-identical, at the branch head.
- **Without `-ffp-contract=off`** (master before and after #526): the 13 cases with default scoring were identical, and the three `--score-min L,-0.7,-0.022` cases differed. Unpaired E. coli differed in 64,546 of 200,000 records, 63,270 of them aligned by one build and not the other. Paired E. coli differed in 216,121 of 400,000 records, and lambda in 1,775 of 20,000.
- **128-bit build:** the `bowtie2-align-s` output of all three builds was identical in all 16 cases, as expected, since nothing here changes that build.

**Tests:**
- **Builds:** the aligners build at each commit on x86-64 Linux (GCC 13.1), `make` succeeds at the branch head there, and `make allall` succeeds at the branch head on macOS arm64.
- **Pytest suite:** 625 passed and 14 skipped on x86-64 Linux (Intel Xeon Gold 6248R) at the branch head, including the 12 new tests; 623 passed and 16 skipped on macOS arm64, where the new tests skip.

**Speed.** This PR should not change speed: the flag affects only scalar floating-point expressions evaluated about once per read, and the dynamic programming is integer SIMD. For context, what #526 gave x86 users is the `-v256` build's speed over the SSE build's. Intel Xeon Gold 6448Y, GRCh38 no-alt index (GENCODE transcripts for RSEM), 250,000 read pairs, median of 3 runs, at the head of #544, from the runs reported there:

| Input | Threads | SSE (s) | `-v256` (s) | Speedup |
|---|---|---|---|---|
| HG002 WGS 2x151 | `-p 1` | 140.9 | 127.4 | 1.11x |
| | `-p 16 --reorder` | 12.1 | 11.0 | 1.09x |
| RNA-seq 2x76 to hg38 | `-p 1` | 49.6 | 49.0 | 1.01x |
| | `-p 16 --reorder` | 4.9 | 4.9 | 1.00x |
| RNA-seq 2x76 to GENCODE transcripts, RSEM settings | `-p 1` | 109.5 | 105.3 | 1.04x |
| | `-p 16 --reorder` | 8.3 | 8.0 | 1.04x |

Before #544, the `-v256` build was no faster than the SSE build on this CPU (0.92x to 1.00x on the same inputs).

**Behaviour changes.**
- **Output on x86-64-v3 CPUs:** since #526, these CPUs run the `-v256` build. For `--score-min`, `--n-ceil` or `-i` values given on the command line that hit a rounding boundary for some read length, its output now matches the 128-bit build's, and so the output of releases before #526. Default settings give the same output as before.
- **`@PG` and `--version`:** on x86-64-v3 CPUs, the `CL:` field of `@PG` and the first line of `--version` name `bowtie2-align-s` (or `-l`) again, instead of the `-v256` binary they have named since #526. Releases before #526 already named `bowtie2-align-s` on nearly every CPU, so for released versions this is no change.

Prepared by Ben Langmead with the help of Claude Opus 5.5, with commits derived from PRs by @jmschrei and @BenjaminDEMAILLE.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94bdbdbeadf4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.