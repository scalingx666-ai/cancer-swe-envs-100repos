# Issue

**Title:** Add a co-gisticChromPlot function gisticChromPlot2v and a helper function yload_gistic

## Description

Dear Author @PoisonAlien,


**Issue**

I want to plot a vensus-plot like coOncoplot, to see only Amp or Del G-scores at a time from two GISTIC objects, showing in a gistic Chromesome plot in a vertical layout.  As we often seen in the published papers like Fig.1D in *Hernandez-Verdin, I. et al. Ann Oncol (2023)*.
<img width="424" alt="image" src="https://github.com/PoisonAlien/maftools/assets/15091103/48073234-5747-456f-ad90-bb121b1c44c0">


So I wrote a new function named `gisticChromPlot2v` and a helper `yload_gistic`. `2` like `2` in `lollipopPlot2` which means this is a comparison of two GISTIC object and `v` means vertical which is discriminative from the original `gisticChromPlot` which takes a horizonal layout.

**Dependecies**
**Depends**
`magrittr`
**Imports**
`dplyr`

**Command**
After adding these functions we can do coGisticChromPlot using the following codes:

```r
# --- load data ---
all.lesions <- system.file("extdata", "all_lesions.conf_99.txt", package = "maftools")
amp.genes <- system.file("extdata", "amp_genes.conf_99.txt", package = "maftools")
del.genes <- system.file("extdata", "del_genes.conf_99.txt", package = "maftools")
scores.gis <- system.file("extdata", "scores.gistic", package = "maftools")

laml.gistic = readGistic(gisticAllLesionsFile = all.lesions, gisticAmpGenesFile = amp.genes, gisticDelGenesFile = del.genes, gisticScoresFile = scores.gis, isTCGA = TRUE)

# or you can use new helper function to quickly read gistic results into new object:
gistic_res_folder = system.file("extdata",package = "maftools")
laml.gistic2 = yload_gistic(gistic_res_folder)
# NOTE:
# to quickly show the plots, I have to use the data inside the maftools, so the left and right part of the plot is mirrored, but
# in practise you will send different GISTIC and MAF objects to `gistic1`,`gistc2`, `maf1`,`maf2`, the function also works well.

laml.maf = system.file('extdata', 'tcga_laml.maf.gz', package = 'maftools') 
laml.clin = system.file('extdata', 'tcga_laml_annot.tsv', package = 'maftools') 
laml = read.maf(maf = laml.maf, clinicalData = laml.clin)
laml2 = laml

# --- plot ---
r = gisticChromPlot2v(gistic1 = laml.gistic, gistic2 = laml.gistic2, type='Del',
                       symmetric = TRUE, g1Name = 'TCGA1',
                       g2Name = 'TCGA2', maf1 = laml, maf2 = laml2, mutGenes = 30)
# will result in
```
![image](https://github.com/PoisonAlien/maftools/assets/15091103/c9d4b9b1-d990-4479-af28-627977dc6126)

```r
r = gisticChromPlot2v(gistic1 = laml.gistic, gistic2 = laml.gistic2,  type='Amp',
                       symmetric = FALSE, g1Name = 'TCGA1',
                       g2Name = 'TCGA2', maf1 = laml, maf2 = laml2, mutGenes = NULL)
# will result in
```
![image](https://github.com/PoisonAlien/maftools/assets/15091103/c3223acd-29c9-40d6-8c33-6326c2bd539a)

To clarify the `symmertric` parameter's function, I'll use my own data to show the differences:
If `symmetric` = `FALSE`, `type` = `'Del'` results in:
![image](https://github.com/PoisonAlien/maftools/assets/15091103/5900cf37-50b3-4e02-b44b-5fbe563b2ee6)
then set `symmetric` = `TRUE` will result to:
![image](https://github.com/PoisonAlien/maftools/assets/15091103/aab89ee1-1d1e-457f-942b-c89372ca39d6)

**This is only the draft, the plot function needs more tests, I'll complete that later**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80b22a48cccb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.