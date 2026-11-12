# Issue

**Title:** Replace snpfilter.sh with a pysam-based snpfilter.py

## Description

`scripts/snpfilter.sh` could not produce a VCF that CNVkit can use for BAF analysis. It called `samtools mpileup --VCF`, an output mode that samtools deprecated in 1.9 and removed in 1.10, and the pileup step never called genotypes in any case. Its pre-filter (`bcftools view --exclude-uncalled --genotype ^miss`) also discarded every record of a sites-only VCF, such as dbSNP or 1000 Genomes common SNPs, which is the input `doc/baf.rst` recommends for tumor-only samples. The Docker image copied the script onto the PATH, but its conda environment never contained samtools or bcftools, so the script could not run there either.

This replaces it with `snpfilter.py`, a console script in `cnvlib/cli/` registered in `[project.scripts]`, which resolves the long-standing TODO in `pyproject.toml` about whether the helper could be installed.

## Behavior

```
snpfilter.py Sample.bam common_snps.vcf.gz -f hg38.fa [-n Normal.bam] [-o Sample.snps.vcf]
```

- Genotyping uses the bcftools bundled in pysam (1.21 with pysam 0.23.3), so no external binary is required and the script works in the Docker image and in CI: `bcftools mpileup -f REF -R SITES -a FORMAT/AD,FORMAT/DP --max-depth 8000 --skip-indels --count-orphans --ignore-RG`, then `bcftools call -m`.
- Per-sample `FORMAT/DP` is requested explicitly. bcftools 1.21 emits `FORMAT/AD` by default but not `FORMAT/DP`, and without per-sample depth the reader falls back to other fields.
- The depth cap is 8000 reads, the samtools mpileup default, because bcftools' default of 250 subsamples reads at deeply sequenced sites and coarsens their allele frequencies.
- `call -m` runs without `--variants-only`. Sites where no sample carries an ALT allele are written with `ALT=.`, which the CNVkit reader ignores. `-A`/`-C alleles` were evaluated and not used: `-A` alone invents spurious ALT alleles under sequencing error, and `-A -C alleles` changed no heterozygous call on test data.
- With a matched normal (`-n`), both BAMs are genotyped jointly and a `##PEDIGREE=<Derived=tumor,Original=normal>` header is written. Without such a header, CNVkit analyzes the first sample of a two-sample VCF unpaired and never consults the normal's genotype.
- Sample names are the BAM filenames without directory or extension, as in CNVkit's other outputs. `--ignore-RG` is required because two BAMs with the same `@RG SM` tag are otherwise merged into one sample column. Two BAMs that would get the same name are refused.
- Only site positions are read, so sites-only VCFs work. Consecutive records at one position, such as split multiallelic records, are collapsed so that each site is genotyped once.
- Sites on chromosomes absent from the BAM or the FASTA are skipped and counted in the log. bcftools fails on a region missing from the BAM, and it silently produces nothing for a chromosome missing from the FASTA. The script raises an error when the BAM and FASTA share no chromosome name, when the sites VCF is empty, or when no site falls on a usable chromosome.
- Sites are processed in blocks of 50,000 because pysam returns each bcftools run's output as a single string, and a whole-genome site list would otherwise be held in memory at once.
- Output goes to `-o` or stdout through `tabio.safe_write`. All input checks and the first block run before the output file is opened, so an input error writes nothing. An `-o` path that is the same file as any input is refused, because the inputs are still being read while the output is written.

## Interface change

The old command was `snpfilter.sh bam vcf [fasta]` and always wrote `calls.vcf` in the working directory. The new command requires `-f`, because bcftools mpileup does not run without a reference, and writes to `-o` or stdout. The name `snpfilter` is kept so that users who know it from #656 and the Docker documentation can find the replacement.

## Behavior worth knowing

- **Tumor-only genotyping drops strongly imbalanced sites at high depth.** On synthetic data, bcftools calls a tumor site with an ALT fraction of 0.8 as `0/1` at 70 reads but `1/1` at 256 or more reads. `load_het_snps` then excludes it, although allelic imbalance is the signal BAF analysis measures. Sites with complete LOH are excluded in tumor-only mode at any depth. `--zygosity-freq 0.1` recovers the imbalanced sites by classifying on allele frequency, and a matched normal avoids the problem, because heterozygous sites are then selected by the normal's genotype. `doc/baf.rst` documents both.
- **PL and QUAL at sites deeper than 255 reads depend on the preceding sites in a run.** Above that depth, htslib computes genotype likelihoods from a random subsample of the bases, drawn from a generator whose state carries over from site to site. Splitting the site list into blocks therefore changes PL and QUAL at deep sites. On the test data, GT, AD and DP, which are the fields CNVkit reads, were identical across block sizes of 7, 13 and 29 in both tumor-only and joint mode. A borderline genotype can vary in the same way within any single bcftools run.

## Other changes

- `Dockerfile`: the `COPY scripts/snpfilter.sh` line is removed. pip installs the console script.
- `doc/baf.rst`: new subsection "Genotyping known SNP sites with `snpfilter.py`" under "Preparing a VCF", beside the Mutect2 `--genotype-germline-sites` route.
- `doc/scripts.rst` and `doc/docker.rst` are updated to match.

## Tests

`test/test_snpfilter.py` builds a synthetic genome at test time with pysam: two contigs, 68 SNP sites, and tumor and normal BAMs at 300 reads per site with exact allele counts. It covers:

- the round trip through `cmdutil.load_het_snps` in tumor-only and paired modes (heterozygous set, finite `alt_freq`, median near 0.5, LOH sites recovered only with the normal, `--zygosity-freq` recovery);
- per-sample GT, AD and DP, with DP not subsampled;
- PEDIGREE pairing and sample naming, including BAMs that share an `@RG SM` tag;
- block invariance of GT, AD and DP;
- skipped contigs, collapsed duplicate positions, an empty sites VCF, and chromosome-name mismatches between the sites, the BAM and the FASTA;
- refusal of colliding sample names and of an output path that is also an input.

Each of the following edits to the implementation makes at least one test fail: dropping the PEDIGREE header, the duplicate collapsing, the BAM or FASTA contig filter, `FORMAT/DP`, the sample renaming, `--ignore-RG`, `--max-depth`, the name-collision check or the same-file check; adding `--variants-only`; or opening the output before validation.

No existing output format or numerical path changes. The full suite passes.

.
.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b72dca19ad82`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.