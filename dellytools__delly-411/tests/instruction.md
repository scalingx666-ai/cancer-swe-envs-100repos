# Issue

**Title:** chore: package python script `delly2bnd` for bioconda recipe

## Description

We realized that we would like to programmatically use `delly2bnd.py` to get correct breakend (`BND`) notation for delly results. We would therefore like to be able to also programmatically install this script via `bioconda`.

This pull request is meant to prepare the script for the respective packaging. It follows [the great documentation and tutorials on python packaging provided by pyOpenSci](https://www.pyopensci.org/python-package-guide/package-structure-code/intro.html).

with the resulting package defintion, the idea is to use a [second `outputs:`](https://docs.conda.io/projects/conda-build/en/stable/resources/define-metadata.html#outputs-section) in the [`meta.yaml` for the `delly` package on `bioconda`](https://github.com/bioconda/bioconda-recipes/blob/ffdea385bba2192459f6d47da3e68eb38fd9838a/recipes/delly/meta.yaml). This would:

1. Make the script installable as a `bioconda` package with the name `delly-scripts`, so `conda install -c conda-forge -c bioconda delly-scripts` would generate an environment in which `delly2bnd` is available as an executable.
2. It would just use the same downloaded tarball that is also used for the main `delly` recipe, and the same version numbers. That way, updating should be automatic and integrated with the packaging of the main `delly` software.

This general setup should also be easy to extend, should you add any further python scripts in the future. I hope you can see our use case and that this is an acceptable setup for you. And please let me know if any of the above needs further explanation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e0bb069465bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.