# Issue

**Title:** One transcript rule for --protein-change too (5.113.0)

## Description

Chained on #607 (5.112.1), which unblocks a master that Varcode 11.11.1 turned red; this branch merges it, so review the first commit and the merge.

Topiary answered "which transcript?" twice. 5.111.6 gave the gene sources one
rule; `--protein-change` kept its own, written years earlier:

| | `--gene-names` / `--gene-ids` / `--cta` / tissues | `--protein-change` |
|---|---|---|
| Rank | complete protein-coding, then standard amino acids, then length | protein length only |
| Ties | lowest transcript ID | nucleotide length, then transcript name |

So `topiary --gene-names EGFR` and `topiary --protein-change EGFR T790M` could
score peptides from different EGFR transcripts in the same release, and the
`--protein-change` door could land on an incomplete translation — the defect
#573 was filed for — because nothing but length was weighed.

**Fix.** The ranking moves into one public function,
`topiary.representative_transcript(transcripts)`. `best_protein_transcript(gene)`
is now that function over a gene's transcripts — same signature, same answers,
same exports — and `protein_change_effects_from_args` applies it to the
transcripts whose protein carries the named reference residue.
`transcript_sort_key` and `best_transcript` are deleted from
`topiary/cli/protein_changes.py`.

Two public names for one rule, not two rules: the gene-wide question and the
question over a candidate set are different questions, and the second is what
`--protein-change` has to ask. Both are documented, exported and tested.

**What changes in practice.** Little. Of eight common substitutions resolved
against both GRCh37.75 and GRCh38.115 (EGFR T790, KRAS G10, TP53 R175, BRAF
V600, PIK3CA H1047, IDH1 R132, NRAS Q61, PTEN R130), fifteen of sixteen keep
the transcript they had. The one that moves is IDH1 R132 on GRCh37, where two
complete 414 aa translations tie and the new tie-break takes IDH1-001
(ENST00000345146) instead of IDH1-006 (ENST00000415913). EGFR T790M still
resolves to EGFR-001 / ENST00000275493, as its test asserts.

**Tests.** `tests/test_twin_conformance.py` now drives all five doors — gene
names, gene IDs, CTA, tissue and `--protein-change` — against one fake gene
whose two translations both carry the named residue, and asserts they reach the
same transcript; parametrized so the complete translation wins when there is
one and the longest wins when there is not. The five unit tests of the deleted
`best_transcript` are replaced by four in `tests/test_cli_coverage.py` that
exercise the CLI door itself: a complete translation beats a longer partial
one, length decides between equally complete ones, transcript ID breaks ties,
and a change whose residue no transcript carries still yields no effect.

`./lint.sh` passes. `./test.sh`: 6374 passed, 3 failed — the three
`test_osteosarc_all_variants` pinned-RNA-outcome cases (ADCY10, IFT122,
PDZRN4), which fail the same way on untouched master in this environment:
they compare against outcomes pinned with Isovar 1.59.1 while 1.90.1 is
installed, and differ only in the predicted effect's offsets
(`p.L1328M` vs `p.L1236M`), which is the test's documented "a different
installed release can legitimately change this" case. CI decides.

This is the second half of the two-rules problem; 5.111.6 (#593) did the first.
Vaxrank still carries two more copies of the choice, in `cta_expression.py` and
`risk_ligand.py`, and can now import this one instead.

https://claude.ai/code/session_01JHJhwj29oDqoeu7TYd34Pi

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `81ad2e0e8b98`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.