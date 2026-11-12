# Issue

**Title:** Weekly refresh: automated pulse, congress abstracts, key-paper citations

## Description

Automated weekly pull of four literature feeds:
- `public/pulse/auto.json`: latest items from journal and news feeds and the FDA OCE page, matched to entity ids (`/pulse/`, automated stream).
- `public/digests/candidates.json`: congress abstracts from the JCO, Annals of Oncology, Blood and Cancer Research supplements via Crossref, matched to products and trials (`/digests/`, candidates for the next digest).
- `public/citations/index.json`: Europe PMC citation counts for every key paper with a DOI or PMID (the "cited N times" chip on paper pages and key-paper cards).
- `public/openalex/papers.json`: OpenAlex citation counts by year for every key paper with a DOI (`/papers/`, most cited key papers).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f350e7fd399c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.