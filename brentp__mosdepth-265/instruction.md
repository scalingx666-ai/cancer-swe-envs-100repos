# Issue

**Title:** Errors in BAM iterator should be fatal

## Description

If this iteration fails, it's likely due to a corrupted BAM (or in my case, an unstable cloud filesystem mount). The current approach of simply logging a message to stderr is suboptimal, since this results in a truncated coverage file and a successful exit code, which is difficult to catch in pipelines. To address this, I've modified `Bam.query()` in `hts-nim` (submitted as a separate pull request ) to raise an exception, which is then caught and handled (with a nonzero exit) in mosdepth.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `52813e08fe57`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.