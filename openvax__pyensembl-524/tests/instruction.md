# Issue

**Title:** Print numbered releases in CLI tables (2.33.1)

## Description

## What was broken

Filtered `pyensembl available` crashed in text mode whenever the selection included a numbered release (2.33.0):

```
$ pyensembl available --species human
  File ".../pyensembl/shell.py", line 509, in _format_table
    widths = [max(len(cell) for cell in column) for column in zip(header, *rows)]
TypeError: object of type 'int' has no len()
```

`--assembly GRCh37` and `--species dog` failed the same way; `--json` worked. The documented commands in the assembly selection guide hit this.

`pyensembl verify --release 75` also failed with the same traceback. That wasn't in the issue: its test used a fake release whose `release` was the string `"75"`, but real numbered releases are ints.

## Cause

`_format_table` called `len()` on every cell, and both `available_catalogue` and `run_verify` pass the release as it is stored: an `int` for numbered releases, a `str` for dated ones.

## Fix

`_format_table` converts each cell with `str` before measuring and padding it. All tables go through it, so `available` and `verify` are both fixed and later tables are covered too. JSON output still has integer releases.

## Verification

- New `test_available_catalogue_text_lists_the_json_releases` (human, GRCh37, dog) checks that the text table lists the same releases and "latest" mark as `--json`. It runs on an empty cache directory, so no network is used. `test_run_verify_reports_status` now uses an int release and checks the table row. Both fail on main with the `TypeError` and pass here.
- Ran by hand: `pyensembl available --species human`, `--assembly GRCh37`, `--species dog`, and `pyensembl verify --release 75`.
- `./lint.sh` clean, `./test.sh`: 1137 passed, and `./docs.sh` (strict) passes.

Version 2.33.1, with release notes. #505 isn't included because no command-line selection reaches the "No genomes selected" branch: every selection resolves to at least one genome, so there was nothing to reproduce.

https://claude.ai/code/session_01JHJhwj29oDqoeu7TYd34Pi

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3219771b395a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.