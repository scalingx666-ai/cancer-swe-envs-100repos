# Issue

**Title:** Document CRAM input and test CRAM/BAM coverage equivalence

## Description

Documents CRAM input, which the code has supported since 6ceef3b but the user documentation never mentioned, and answers the BAM/CRAM/BQSR questions in #1105 with tests behind each claim.

## Documentation

A new "CRAM input" subsection under `coverage` in `doc/pipeline.rst` covers:

- **`-f/--fasta` is needed to read CRAM.** It is accepted by `batch` (including together with `-r/--reference`), `coverage` and `autobin`, and is forwarded to every pysam and `samtools bedcov` call. CNVkit builds a missing `.crai` itself; CRAM indexing does not require the reference.
- **What happens without it.** HTSlib searches the `-f` FASTA (by name), then `REF_CACHE` and `REF_PATH` (by the `@SQ` `M5` checksum), then the local file in the `@SQ` `UR` tag (by name). When `REF_PATH` is unset, HTSlib 1.21 and earlier (pysam 0.23 and earlier) fall back to the EBI reference server and download every sequence into `~/.cache/hts-ref`. This is the multi-hour slowdown reported in #759. HTSlib 1.22 and later (pysam 0.24 and later) removed that default and stop with `Unable to fetch reference` / `Failure to decode slice`. When `REF_PATH` is set but lacks the sequence, every version fails the same way. The `[W::cram_populate_ref] Creating reference cache directory` warning appears only when HTSlib creates the default cache directory, so its absence does not rule out downloads.
- **Local reference caches.** `seq_cache_populate.pl` prints the template to assign to `REF_CACHE`. The documentation recommends assigning the same template to `REF_PATH`, because HTSlib 1.21 selects the EBI default whenever `REF_PATH` is empty, regardless of `REF_CACHE`.
- **Mismatched FASTA.** Bases that differ within a decoded slice give `MD5 checksum reference mismatch`, and CNVkit stops with an error rather than computing coverage from misread alignments. A `@SQ length mismatch` warning, as in #779, indicates a different assembly. It can appear while decoding still succeeds, when the sequences agree over the region read, so it should be read as a sign of the wrong FASTA.
- **Equivalence.** A CRAM and a BAM holding the same alignments yield identical coverage in both the default and `--count` paths. Duplicate flags are honored in CRAM exactly as in BAM. A new paragraph beside the existing duplicate-marking note states that CNVkit does not read base qualities. GATK ApplyBQSR rewrites only those scores, and apart from GATK's default filter for malformed reads (`WellformedReadFilter`, the only default on `ReadWalker`), it passes every read through unchanged. MarkDuplicates output and recalibrated output therefore yield the same coverage. For the same reason, CRAM's lossy quality-score compression has no effect on coverage.

The HTSlib behavior above was checked against the `cram_populate_ref` source at the 1.21 and 1.22 tags. It was also observed directly with pysam 0.23.3 (HTSlib 1.21) and pysam 0.24.1 (HTSlib 1.24).

## Tests

- `test_coverage_skips_marked_duplicates` now runs on CRAM as well as BAM, and half of the surviving reads carry Phred 2 at every base. The CRAM is written against a FASTA that is then renamed, so its `UR` tag names a missing file. Reads run with `REF_PATH` and `REF_CACHE` pointed at a nonexistent directory, so decoding depends only on the FASTA passed to CNVkit, and no request ever goes to the network. This isolation is necessary: the synthetic poly-A sequence is served by the EBI MD5 server, and without it, a version of `bedcov` that dropped the FASTA still passed.
- `test_coverage_cram` now requires exact equality, in both the depth and `--count` paths. It previously used `assert_array_almost_equal(..., decimal=1)` on the depth path only. The measured difference is 0.0. The fixture has two duplicate-flagged reads inside the chrM target; counting them would raise that target's depth from 82.5 to 84.5. The row count is now asserted absolutely, so an empty result cannot pass. The fixture CRAM's `UR` tags name an absolute path on the machine that wrote it, so where that path exists, this test cannot show that the FASTA argument is used. The docstring notes this, and points to the test above, which does show it.

Each test was checked against a deliberately broken implementation:

| Mutation | Result |
|---|---|
| `bedcov` counts duplicates for CRAM only | `test_coverage_cram (by_count=False)` and the `fmt='cram'` subtest fail; BAM subtests pass |
| `bedcov` no longer receives the FASTA | `fmt='cram'` subtest fails; BAM subtest passes |

Both tests pass under pysam 0.23.3 (the minimum pin) and pysam 0.24.1.

## CLI help

The `batch`, `coverage` and `autobin` command summaries and input arguments, and `batch -n/--normal`, now name CRAM. Previously they named only BAM and bedGraph.

## Not changed

- No numerical output or file format changes. Only documentation, tests, and help text are modified.
- When a CRAM cannot be decoded, CNVkit currently reports `OSError: truncated file`, which suggests file corruption rather than a missing or mismatched `-f`. This is documented here through the HTSlib messages that precede it; a clearer error message is left for a separate change.
- The `autobin` positional help has claimed bedGraph input since 83c16bb, but `autobin` calls `ensure_bam_index` unconditionally and fails on `.bed.gz`. This is left for a separate change, because it requires choosing between rejecting bedGraph input and supporting it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4509b4f4582`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.