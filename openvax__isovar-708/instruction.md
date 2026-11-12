# Issue

**Title:** Reuse read alignment geometry across coding models

## Description

`aligned_reference_prefix` runs once per RNA alternative and ORF model. Each call rescanned every supporting read base by base, and summed all of the transcript's exons for every aligned base. The read geometry depends only on the sequence, so the cost grew with alternatives × models × reads without adding information (#690).

## Change

Each piece of work moves to the object that owns it, instead of a cache threaded through three call layers:

- **`VariantSequence.aligned_positions()`** returns the genomic placements its reads observe at each sequence offset. It is memoized like `coverage()`, because a sequence is immutable after construction. Each offset maps to a set, which keeps three cases distinct: an inserted base (`None`), an unobserved offset (absent) and disagreeing reads (more than one placement). That lets each strand's upstream flank be judged exactly as before.
- **`SplicedCoordinates`** maps genomic coordinates to strand-oriented transcript offsets by binary search over exon starts. It is cheap enough to build per call, so it needs no cache and no cache key.
- **`aligned_reference_prefix`** keeps only the per-model frame logic.

Private `VariantSequence` slots are now excluded from value identity by rule (leading underscore) instead of by name.

## Results are unchanged

- No RNA or model alternative is dropped or capped.
- A differential fuzz against the previous implementation agrees on 65,705 contexts (20,849 resolved, 44,856 rejected). It covers both strands, one-base and adjacent exons, insertions, deletions, conflicting and unaligned reads, sequences trimmed shorter than their reads, start-codon contexts and multi-transcript contexts.
- `reconstruct_variant_orfs` output is byte-identical on a synthetic 40-exon case with 10,620 protein candidates.

## Speed

One RNA alternative against 100 ORF contexts, synthetic data:

| Scenario | Before | After | Speedup |
|---|---|---|---|
| 60 exons, 500 reads | 822 ms | 20 ms | 42× |
| 360 exons, 500 reads | 1,792 ms | 24 ms | 75× |
| 60 exons, 5,000 reads | 6,293 ms | 94 ms | 67× |
| 60 exons, 500 reads, minus strand | 1,202 ms | 21 ms | 58× |

End to end through `reconstruct_variant_orfs` (400 reads, 60 models, 177 RNA alternatives), wall time falls from 77 s to 38 s. The remaining time is mostly `coding_interval_support`, which has the same per-model read rescan; that is filed separately as #707.

## Tests

- `aligned_positions()` semantics: insertion, splice, unaligned and out-of-range bases, disagreeing reads, memoization.
- `SplicedCoordinates` against a brute-force oracle on both strands, with unsorted, adjacent and one-base exons.
- Five alternative coding models of one sequence traverse each read's blocks once. The previous implementation traverses them 20 times for 4 reads.

Bump to 1.90.2. `./lint.sh` passes; full `./test.sh` and Python 3.11–3.14 CI gate the merge, then clean-master `./deploy.sh`.

.

https://claude.ai/code/session_01JHJhwiEVBQqE7W5kKcCQ4p

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ade895c23365`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.