# Issue

**Title:** Re-pin three effects for Varcode 11.11.1's restored tie-breakers (5.112.1)

## Description

Master is red, and no topiary change caused it. Varcode 11.11.1 was published
at 02:26 UTC today and Isovar 1.90.1 at 03:06; master's last green run was
20:55 yesterday. Every run since resolves the new Varcode and fails the same
three cases of `test_every_literal_allele_has_a_checked_rna_and_consumer_outcome`:

```
{'predicted_effect': 'p.L1328M'} != {'predicted_effect': 'p.L1236M'}   ADCY10-chr1-167824546
{'predicted_effect': 'p.R648H'}  != {'predicted_effect': 'p.R849H'}    IFT122-chr3-129502728
{'predicted_effect': 'p.E677V'}  != {'predicted_effect': 'p.E417V'}    PDZRN4-chr12-41572809
```

**Cause.** ,
released in 11.11.1: since 2024 the helper behind `top_priority_effect`'s
transcript-name tie-breakers returned `""` for named transcripts, so those
tie-breakers never applied and a tie went to whichever effect came first.
They work again. Each of these three variants has several transcripts whose
effects tie on class, biotype, completeness, CDS length, transcript length and
exon count, so each is now reported on the lower-numbered one. The substituted
residue is the same in both; only which transcript numbers it changes. This is
the test's own documented case: "If the installed release changed this
evidence, re-run the audit and re-pin."

**Fix.** A `VARCODE_11_11_1_COMPARISONS` table beside the existing
`VARCODE11_COMPARISONS`, applied from 11.11.1 on, the same way topiary already
carries Varcode 10.21.2, 10.21.3 and 11 vocabularies without losing the earlier
ones. Nothing in `expected.json` is rewritten: the RNA evidence — reads,
fragments, filters, status, sequences — stays pinned to the original
observation, and a reader can still see what each release reported.

`test_restored_tie_breakers_move_only_the_reported_effect` asserts that from
both sides of the release: for each of the three, the effect at 11.11.0 differs
from the effect at 11.11.1, the effect class is unchanged, the new offsets are
the ones recorded, and every other field of the outcome is identical.

**Verified.** `./lint.sh` passes. `tests/test_osteosarc_all_variants.py` passes
in full against Varcode 11.11.1 and Isovar 1.90.1, where it fails three cases
on master in the same environment.

#606 is blocked behind this and will rebase on it.

https://claude.ai/code/session_01JHJhwj29oDqoeu7TYd34Pi

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `186365d8a2cb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.