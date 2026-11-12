# Issue

**Title:** Error on input paths that match no file instead of silently dropping them

## Description

## Problem

`update_with_glob` expands the first 10 positional paths with `walkFiles`. A path that does not exist expands to nothing, so it is silently dropped. As a result, `somalier relate` (and `ancestry` / `contamination`, which use the same helper) runs on fewer samples than requested and exits 0 without a warning:

```
$ somalier relate -o out ex/*.somalier ex/DOES_NOT_EXIST.somalier
[somalier] starting read of 1 samples
...
$ echo $?
0
```

A typo in a sample path therefore gives a result that is missing that sample, with nothing in the output to show it. Paths after the 10th are not globbed and do fail later with an IOError, so the behavior also depends on where the path is in the argument list.

## Fix

When an argument (literal path or glob pattern) matches no file, quit with an error:

```
$ somalier relate -o out ex/*.somalier ex/DOES_NOT_EXIST.somalier
[somalier] no file found matching: ex/DOES_NOT_EXIST.somalier
$ echo $?
1
```

Valid paths and quoted glob patterns work as before. I added a functional test (`check_relate_missing_file`). `tests/functional-tests.sh` passes locally (32/32).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `edf6c917d97b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.