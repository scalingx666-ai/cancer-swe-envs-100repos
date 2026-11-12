# Issue

**Title:** STAR-RSEM pipeline

## Description

I have added the following features to RSEM:
- Preparing STAR's genome indices by **rsem-prepare-reference**, with options:
  - **--star**: ask RSEM to prepare genome indicies
  - **--star-path <path>**: path of STAR's executable file
  - **--star-sjdboverhang <int>**: length of the genomic sequence around STAR's annotated junction
  - **-p/--num-threads <int>**: number of threads for running STAR
- Aligning reads by STAR in **rsem-calculate-expression**, with options:
  - **--star**: ask RSEM to align reads by STAR
  - **--star-path <path>**: path of STAR's executable file
  - **--gzipped-read-file**: input read file(s) is compressed by gzip
  - **--bzipped-read-file**: input read file(s) is compressed by bzip2
  - **--output-star-genome-bam**: to output STAR's genome bam file, named 'sample_name.STAR.genome.bam' under the same folder as RSEM's results
  - **--sort-bam-by-read-name**: to sort BAM file aligned under transcript coordinate by read's name
  - **--sort-bam-buffer-size <string>**: size of memory buffer for sorting BAM file
- Parameters for running STAR are from ENCODE3's STAR-RSEM pipeline
- Documented above new options and STAR's outputs. Examples are added as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffbb65f0deb6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.