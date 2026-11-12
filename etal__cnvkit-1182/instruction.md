# Issue

**Title:** Estimate autobin depths from bedGraph input

## Description

`autobin`, and `batch` whenever it sizes bins automatically, failed on bedGraph (`.bed.gz`) input with "file does not contain alignment data", although the help of all three commands lists bedGraph since it became a `coverage` input (#984). This adds bedGraph support to `autobin` for all three protocols, fixes the opening of CSI-indexed bedGraph files, and fixes a sampler edge case found along the way.

## How depths are estimated from a bedGraph

For a BAM or CRAM file, `autobin` reads per-chromosome read counts from the index and estimates the read length; the wgs depth and the hybrid off-target depth come from those. A bedGraph records neither, nor chromosome lengths. Every bedGraph depth is therefore measured in the same seeded sample of regions (`sample_midsize_regions`) that the on-target depth already uses, reading the bedGraph through `coverage.bedgraph_to_basecount`:

| Method | Regions sampled from a bedGraph |
|---|---|
| amplicon | targets |
| hybrid, on-target | all targets |
| hybrid, off-target | accessible regions on targeted chromosomes, minus the targets |
| wgs | accessible regions (required; a clear `ValueError` otherwise) |

Each sampled region is trimmed to a centered 10 kb window (`BEDGRAPH_SAMPLE_WIDTH`). Accessible and off-target regions reach megabases each: 100 midsize regions of the hg19 access file span about 49 Mb, and `bedgraph_to_basecount` parses lines in Python at roughly 0.2 to 1.3 million per second, so untrimmed sampling of a 30x per-base WGS bedGraph would take minutes. Trimmed, a sample reads about 1 Mb. Targets are almost always shorter than the window, so the on-target estimate is unaffected by it.

The bedGraph hybrid on-target depth covers all targets. The BAM path restricts it to chromosomes shared with the accessible regions only because it subtracts each chromosome's target reads from that chromosome's total; for a bedGraph the restriction would discard data, and with accessible regions from `access` (which omits chrM and alternative contigs) it would drop chrM targets or, for a mitochondrial panel, fail with the chromosome-name error.

Without accessible regions, or with none off-target on a targeted chromosome, hybrid on a bedGraph estimates only the on-target bin size and logs a warning. In `batch`, a hybrid bedGraph normal without `--access` takes its accessible regions from `--fasta`, as `batch` already does for wgs; with neither, the default off-target bin size applies.

A chromosome absent from the bedGraph counts as zero depth, as it does in `coverage`.

### Measurements

On the test sample (`na12878-chrM-Y-trunc`), `my-targets.bed`, amplicon:

| Input | Depth | Bin size |
|---|---|---|
| BAM | 50.341 | 1986 |
| bedGraph (fixture, includes duplicate reads) | 51.500 | 1942 |
| bedGraph built with `samtools depth -J -G 0x704` | 50.341 | 1986 |

The fixture bedGraph counts the duplicate-flagged reads that the BAM path excludes; a bedGraph of the same reads gives the identical estimate, since both use the same seeded region sample. A synthetic bedGraph with known depths (300x targets, 30x elsewhere) yields exactly 300 and 30 for the on- and off-target depths, and 30 for wgs.

The off-target estimators differ in kind (BAM subtracts target reads from index totals; a bedGraph is measured directly), so their values are not expected to agree closely.

## CSI-indexed bedGraph files

`find_bedgraph_index` (formerly `validate_bedgraph_index`) accepted a `.csi` index, but the file was then opened with `pysam.TabixFile`'s default lookup, which finds only `.tbi`. mosdepth indexes its per-base output with CSI, so that output passed the check and failed to open in `coverage` as well. The located index is now passed to `TabixFile` explicitly.

## Sampler edge case

`sample_midsize_regions` keeps regions between the 25th and 75th percentile of size. With exactly two regions of different size, both percentiles fall strictly between the two sizes and nothing is kept, which surfaced as the chromosome-name error (#421). All regions of nonzero size are now eligible in that case. A randomized comparison over several thousand region sets confirmed that every set which previously kept regions keeps exactly the same ones.

## Behavior changes for BAM and CRAM

No BAM or CRAM depth or bin size changes for input that previously succeeded: `do_autobin` output was compared exactly against master across seven region sets, and the frozen regression test passes. Three error cases now behave differently for every input type:

- A region set with no region of nonzero width (for example, targets and accessible regions sharing no chromosome names) reaches the chromosome-name error instead of an `IndexError`.
- An empty set of accessible regions passed to `do_autobin` is treated as none given, as the command line already did; BAM hybrid then uses the index instead of failing.
- Two regions of different size, which raised the chromosome-name error, now yield a depth.

## Documentation

`doc/pipeline.rst` gains a "bedGraph input" section (tabix index requirement, ignored read-selection options, missing chromosomes, and the read filters to apply for coverage comparable to a BAM, including mosdepth's defaults), and the `autobin`, `coverage`, and `batch` sections describe the bedGraph behavior. The `autobin` help for the positional argument and `-g` is updated. The `coverage` module docstring, which named the `antitarget` command, is corrected.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94bf55ae5646`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.