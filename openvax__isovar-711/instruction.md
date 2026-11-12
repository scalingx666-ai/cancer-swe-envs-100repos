# Issue

**Title:** Store each distinct read set once per export

## Description

`EvidenceSets.support` rebuilt the stored evidence set on every call. Building it hashes every read ID through canonical JSON and SHA-256, so an export that describes many hypotheses over the same reads spent most of its time re-hashing them (#710).

## Change

`EvidenceSets` remembers the evidence set ID of each distinct set of read identities and builds the stored set once. `support` and `quality_support` both go through the one `_store` method, which replaces two copies of the build-and-store lines.

The scope is fixed per instance, so the ID is a pure function of the identity set. Support counts, cell/UMI fields and base-quality partitions are still computed on every call, and each call still returns its own record.

## Results are unchanged

- The stored `sets`, their order and every `evidence_set_id` are the same as before.
- `reconstruct_variant_orfs` output is byte-identical on the synthetic case below (10,620 protein candidates).
- On real COLO829 RNA at the DYNLL2 locus (45 alt reads, 180 RNA candidates, 150 protein candidates), the full output is byte-identical across 1.90.1, 1.90.3 and this branch.

## Speed

`reconstruct_variant_orfs` on a synthetic locus with 400 reads carrying SAM segment identities, 60 ORF models and 177 RNA alternatives: wall time falls from 49.5 s to 7.9 s. The benchmarks for #690 and #707 used reads without identities, which skip the hashing, so they did not show this cost. The remaining profile is flat; no single function takes more than about a fifth of it.

## Tests

- A read set repeated in another order, or seen through another placement, is built once, and the stored sets equal a fresh build.
- Records returned for a shared set are independent objects.
- Reads without identities still store no set.

Bump to 1.90.4. `./lint.sh` passes; full `./test.sh` and Python 3.11–3.14 CI gate the merge, then clean-master `./deploy.sh`.

.

https://claude.ai/code/session_01JHJhwiEVBQqE7W5kKcCQ4p

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8091b217fb7f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.