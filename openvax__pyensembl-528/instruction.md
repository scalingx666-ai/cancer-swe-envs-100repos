# Issue

**Title:** Model stop codons completed by polyadenylation (2.34.0)

## Description

. Needed by openvax/varcode#731.

Six human mitochondrial reading frames end in `T` or `TA`, and the poly(A) tail completes a `TAA` stop codon. NCBI's [NC_012920.1](https://www.ncbi.nlm.nih.gov/nuccore/NC_012920.1) notes this on exactly MT-ND1, MT-ND2, MT-CO3, MT-ND3, MT-ND4 and MT-CYB. Ensembl annotates no stop codon for them, so `coding_sequence` and `three_prime_utr_sequence` were `None`, and varcode reports every variant in them as `IncompleteTranscript`, including the LHON mutations m.3460G>A (ND1) and m.11778G>A (ND4).

## Change (2.34.0)

- New `Transcript.stop_codon_completed_by_polyadenylation`. True when all of these hold:
  - the transcript is on a mitochondrial contig;
  - its genetic code reads `TAA` as a stop. That excludes NCBI tables 14 and 33, where `TAA` is tyrosine; checked against NCBI's `gc.prt`;
  - it has a three-base start codon and no `stop_codon` row;
  - its CDS reaches the transcript's 3' end, one or two bases past the last full codon, in `T` or `TA`.

  It is False when the transcript sequence is unavailable.
- For those transcripts:
  - `coding_sequence` runs from the start codon to the end of the transcript, ending in the `T` or `TA`;
  - `three_prime_utr_sequence` is `""`.
- `complete` is unchanged and stays False for them. Downstream code reads `complete` as "has annotated three-base start and stop codons", and calls `stop_codon_positions` and `stop_codon_spliced_offsets` after checking it. Those still raise for these transcripts, because no genomic base carries the final `A`. A caller that wants these transcripts checks the new property and appends `A` until the length is divisible by three.
- Every other transcript without a stop codon is unchanged: nuclear fragments, CDSs followed by a 3' UTR, and partial codons other than `T` or `TA`.
- Docs: a "Stop codons completed by polyadenylation" section in the transcripts guide, with an MT-ND1 example checked against Ensembl 93. Also a release note.

## Tests

`tests/test_polyadenylation_stop_codon.py`:
- **Real data:** Ensembl 75 and 93 flag exactly the six genes. For each, the coding sequence completes to `TAA` and has as many codons as the Ensembl protein. The other seven are `complete`.
- **Offline tiny-GTF cases:**
  - flagged: `T`, `TA` and lowercase, on both strands;
  - not flagged: `AG`, a full final codon, an unannotated full `TAA`, a 3' UTR, a nuclear contig, no start codon, tables 14 and 33, and no sequence.
  - Disabling any one condition in the code fails the case that names it.

Results:
- `./lint.sh`: passes.
- `./test.sh`: 1161 passed.
- `scripts/check_docs_examples.py`: 18 examples pass against Ensembl 93.
- varcode 11.13.0, unchanged, on this branch gives the same effects as on 2.33.2 for MT variants. Its mitochondrial, annotator, reference and variant tests pass (854).
- CI: green on 6510cbf (Python 3.11–3.14, source distribution, release-version check).

## Not covered

- An MT contig named by RefSeq accession (`NC_012920.1`) isn't recognised as mitochondrial unless the reference metadata declares it. That is pre-existing; the same contig also gets nuclear table 1. Ensembl GTFs use `MT`.

https://claude.ai/code/session_01JHJhwgzAgwTefwxNLB1ztR

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `39cc1394de64`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.