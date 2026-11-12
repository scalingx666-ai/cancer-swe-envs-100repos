# Issue

**Title:** Place supporting reads once per RNA sequence, not once per ORF model

## Description

`coding_interval_support` runs once per RNA alternative and ORF model. Each call reverse-complemented and compared every supporting read with the candidate cDNA, then added the read's fragment identities to one set per covered base. Where a read lies on a cDNA depends only on the cDNA, the focal allele offset and the strand, so the cost grew with alternatives × models × reads × read length without adding information (#707).

## Change

- **`AlleleReadTiling`** places each read on a candidate cDNA once. It records the read's span and the offsets where it disagrees with the cDNA. `interval_support(interval, support)` then answers for any coding interval.
- **Depth:** fragments whose reads all agree with the cDNA share one precomputed depth profile, so an interval's depth is a slice. A fragment with a mismatched read is admitted per interval, because a mismatch outside the interval does not exclude the read. This keeps the previous rule exactly.
- **`reconstruct_variant_orfs`** shares one tiling per RNA sequence and strand across its ORF models. `_candidate` reads the oriented cDNA from the tiling instead of orienting it a second time.
- **`coding_interval_support`** stays as the single-interval form. Its other callers, `protein_hypotheses` (once per translation) and `patient_background` (once per translation and transcript), do not repeat work at this scale, so they are unchanged.

## Results are unchanged

- No RNA or model alternative is dropped or capped.
- A differential fuzz against the previous implementation agrees on 96,000 intervals over 8,000 random read sets, 6,728 of which contain mismatched reads. It covers both strands, reads that overhang the cDNA, empty intervals, merged and single observations, legacy reads without identities, reads with several segment identities, and fragments shared between reads. The stored evidence sets also agree.
- `reconstruct_variant_orfs` output is byte-identical on the synthetic 40-exon case from #690 (10,620 protein candidates).

## Speed

End to end through `reconstruct_variant_orfs` (400 reads, 60 ORF models, 177 RNA alternatives), wall time falls from 38 s to 7.5 s. Under cProfile, coding-interval support falls from 118 s to 11 s cumulative. Most of what remains there is `EvidenceSets.support` building the three span records for each candidate.

## Tests

- A mismatch excludes a read only from intervals that contain it, and a fragment with one clean and one mismatched read counts once.
- One tiling answers every interval of a cDNA like a fresh measurement and like a brute-force definition of depth, on both strands.
- Four ORF models of one locus build as many tilings as one model does.
- Invalid strand, focal offset and interval are rejected.

Bump to 1.90.3. `./lint.sh` passes; full `./test.sh` and Python 3.11–3.14 CI gate the merge, then clean-master `./deploy.sh`.

.

https://claude.ai/code/session_01JHJhwiEVBQqE7W5kKcCQ4p

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bb1eb67af0ed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.