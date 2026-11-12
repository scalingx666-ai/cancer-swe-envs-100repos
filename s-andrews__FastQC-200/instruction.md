# Issue

**Title:** Make SVG plots the default in Java and fix CI snapshot tests

## Description

CI has been failing on every PR (and on pushes to master) since around April, failing on `FileContentsTest` with diffs in the base64-encoded PNG plot images inside the snapshot HTML. This is due to differences in the text with anti-aliased pixels, so the byte comparison fails. 

This should have been fixed by using SVG plots by default instead of PNG. However, PR #191 flipped the default in the perl launcher (`fastqc` adds `-Dfastqc.svg=true` unless `--png` is passed) but not in the Java code itself, where `FastQCConfig.svg_output` was still initialised to `false`. The integration tests invoke `java uk.ac.babraham.FastQC.FastQCApplication` directly, bypassing the launcher, so they got the Java default (PNG) and have been comparing pixel-perfect PNGs ever since.

This PR:

- **`FastQCConfig.java`**: default `svg_output = true`. The `-Dfastqc.svg` handler now honours both `true` and `false`, so callers can still opt back into PNG.
- **`fastqc` launcher**: only pass `-Dfastqc.svg=false` when `--png` is set without an explicit `--svg`. SVG is now the default everywhere.
- **`FileContentsTest_{minimal,complex}_fastqc_report.approved.html`**: regenerated. Plots are now `data:image/svg+xml;base64,...` instead of `data:image/png;base64,...`. SVG output is deterministic.
- **`VersionTest.java`**: assertion references `FastQCApplication.VERSION` instead of the hardcoded `v0.12.1`, so it continues to pass after future version bumps.

Tested locally with Java 11 (matches CI) and Java 17: unit + integration suites pass clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b6bc4b908b6c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.