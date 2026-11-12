# Issue

**Title:** Migrate CI workflow from apt-get to conda/miniforge

## Description

The CI workflow was using apt-get to install dependencies, which provided limited versions and missing packages. This migrates to conda/miniforge for better bioinformatics tooling support.

## Changes

- **Setup conda environment**: Added `conda-incubator/setup-miniconda@v3` with miniforge, using conda-forge and bioconda channels with strict priority
- **Install all dependencies via conda**: Replaced apt-get with conda install for samtools, bcftools, freebayes, bedtools, bwa, minimap2, parallel, vcflib, vt, snpeff, seqtk, snp-sites, wgsim, any2fasta, perl-bioperl, and samclip
- **Shell configuration**: Added `shell: bash -el {0}` to all steps to ensure conda environment activation

```yaml
- name: Setup Miniforge
  uses: conda-incubator/setup-miniconda@v3
  with:
    miniforge-version: latest
    activate-environment: snippy-env
    channels: conda-forge,bioconda
    channel-priority: strict

- name: Install Dependencies
  shell: bash -el {0}
  run: |
    conda install -y \
      samtools bcftools freebayes bedtools \
      bwa minimap2 parallel vcflib vt snpeff \
      seqtk snp-sites wgsim any2fasta \
      perl-bioperl samclip
```

<!-- START COPILOT ORIGINAL PROMPT -->

<details>

<summary>Original prompt</summary>

>
> ----
>
> *This section details on the original issue you should resolve*
>
> <issue_title>Create a CI workflow using conda</issue_title>
> <issue_description>Please fix the CI workflow
> It should enable conda and miniforge
> And install samtools, bcftools, freebayes, etc
> </issue_description>
>
> ## Comments on the Issue (you are @copilot in this section)
>
> <comments>
> </comments>
>

</details>

<!-- START COPILOT CODING AGENT SUFFIX -->

- Fixes tseemann/snippy#623

<!-- START COPILOT CODING AGENT TIPS -->
---

💡 You can make Copilot smarter by setting up custom instructions, customizing its development environment and configuring Model Context Protocol (MCP) servers. Learn more [Copilot coding agent tips](https://gh.io/copilot-coding-agent-tips) in the docs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f7d0acb0dc20`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.