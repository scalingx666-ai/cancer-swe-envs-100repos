# Issue

**Title:** Editing getBarcodeInfo function to handle updates GDC database for TCGA data

## Description

TCGA data has been having its clinical data incorrectly parsed due to a GDC database update that occurred on 1/30/2025 (see discussion ). The primary issue seems to be due to additional "nodes" being added to TCGA cases that had multiple diagnoses beyond the primary diagnosis. 

During the getBarcodeInfo() step of gdcPrepare(), the api derived search results are filtered using a match()-based strategy that results in only the first row of object df per barcode being included being included in the returned data. 

My edits create a column called "diagnosis_id" which is used to sort the df object so that when the filtering occurs, it keeps the primary diagnosis (which is what best describes the associated sequencing data typically). This is as opposed to the previous method which sometimes kept the non-primary diagnoses (indicated by values appended with a number in df$diagnosis_id). The column "diagnosis_id" is then removed for backwards consistency (and due to it now being redundant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2cea937cd54d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.