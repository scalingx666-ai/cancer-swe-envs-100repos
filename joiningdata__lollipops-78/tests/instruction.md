# Issue

**Title:** Alternative source of protein domains

## Description

`lollipop` fetches Pfam domains and additional motif features using the InterPro REST API. If there are no Pfam domains or motif features, the REST API response will return an HTTP status of 204 (No Content). Currently, the code checks that responses have an HTTP status of 200 and exits if they don't. This pull request changes this behavior to accepts 204 HTTP responses (although no regions will be drawn in this case).

**Example for `CCDC61` (no Pfam annotation)**

```sh
./lollipops -legend -show-motifs -show-disordered -w=1400 -o CCDC61.png CCDC61 R21Q Q63T G102A
```

![CCDC61](https://github.com/joiningdata/lollipops/assets/2097501/e8165a98-705b-4346-b226-cdb6cd3d7688)

---

This pull request introduces a new option, `-D <source>`, which enables the customization of the domain source. By default, it is set to _Pfam_ (consistent with the current behavior). However, it also accepts _InterPro_ as a source. In the case of InterPro, representative domains, selected from all domain and repeat databases within the InterPro consortium (including CDD, NCBIfam, Pfam, PROSITE, and SMART), are retrieved and then drawn.

**Example for `CCDC61` (no Pfam annotation, but CDD annotation)**

```sh
./lollipops -legend -show-motifs -show-disordered -D interpro -w=1400 -o CCDC61.png CCDC61 R21Q Q63T G102A
```

![CCDC61](https://github.com/joiningdata/lollipops/assets/2097501/91f0e2d9-816b-466d-be1f-a2a8c6eeb90c)

There are cases where two databases of protein families and domains describe the same domain, but with slightly different annotation locations. The use of `-D interpro` allows for the drawing of the "best" domain, typically the longest one.

Example with `PIK3CA`:

```sh
./lollipops -labels -legend -D pfam -w=1400 -o PIK3CA-Pfam.png PIK3CA N345K
```

![PIK3CA-Pfam](https://github.com/joiningdata/lollipops/assets/2097501/bdaa58bf-b4de-4ffb-8f33-c7d625a36197)

The Pfam PI3K-type C2 domain does not include the N345K variant.

```sh
./lollipops -labels -legend -D interpro -w=1400 -o PIK3CA-InterPro.png PIK3CA N345K
```

![PIK3CA-InterPro](https://github.com/joiningdata/lollipops/assets/2097501/61eaa8b0-c3ea-4f20-8b13-e8aac26929e7)

However, the same domain described by CDD does.

---

Finally, in cases where the `-o <filename>` option is not provided, the graphic is saved as `<gene-symbol>.svg`. However, if a UniProtKB accession is used instead of a gene symbol (`-U <accession>`), the graphic is saved as `.svg`. This pull request resolves this issue by using the UniProt accession for the output file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f835f4b9ffd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.