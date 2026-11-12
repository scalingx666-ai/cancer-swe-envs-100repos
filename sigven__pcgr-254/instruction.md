# Issue

**Title:** move from bump2version to bump-my-version

## Description

Switching to https://github.com/callowayproject/bump-my-version since bump2version is deprecated (https://github.com/c4urself/bump2version/tree/master?tab=readme-ov-file#bump2version).

Example use:

```
$ bump-my-version show-bump
2.1.2 ── bump ─┬─ major ─ 3.0.0
               ├─ minor ─ 2.2.0
               ├─ patch ─ 2.1.3
               ╰─ dev ─── 2.1.2.1
               
$ bump-my-version bump --new-version 2.1.2.9000 --dry-run -v
[...]
[lots of verbose output showing which files will change and how]

$ bump-my-version bump --new-version 2.1.2.9000
$ bump-my-version bump dev # or patch, minor, major
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `14400cf2857c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.