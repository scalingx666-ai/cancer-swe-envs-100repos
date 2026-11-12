# Issue

**Title:** Confine BundleAlgo.export_to_disk to the output directory

## Description

### Description

`BundleAlgo.export_to_disk(output_path, algo_name)` joined `algo_name` to `output_path` and then removed and recreated that directory. The name was never checked, so a name containing a path separator, `..`, or an absolute path, or an existing entry that is a symlink, could resolve somewhere other than a child of `output_path`. The Algo's `name` attribute, used as the template sub-directory to copy from, was not checked either.

This PR:
- requires `algo_name` to be a single directory name (no separators, no drive component, no NUL, not empty, `.` or `..`);
- applies the same check to a non-empty Algo `name` (an empty name still copies the whole `template_path`, as before);
- checks that the template folder exists, then resolves the destination with `realpath` and requires it to be inside `output_path`, before anything is removed or copied.

All built-in callers (`BundleGen.generate`, `NNIGen`, `OptunaGen`) already pass single directory names, so their behaviour does not change.

### Tests
- New `tests/auto3dseg/test_export_to_disk.py`. It covers a normal export, re-exporting over an existing directory, rejected names (`../x`, absolute paths, `a/b`, `.`, `..`, empty), a symlinked destination that points outside `output_path`, and a rejected Algo `name`. In every rejected case it also checks that the neighbouring directory is untouched. 9 of these fail on `dev`.
- `tests/auto3dseg/` passes locally (24 passed).

### Types of changes
- [x] Non-breaking change (fix or new feature that would not break existing functionality).
- [x] New tests added to cover the changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e9b19ccb519`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.