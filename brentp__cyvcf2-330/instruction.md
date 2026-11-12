# Issue

**Title:** Update wheel workflow PyPy selectors to pp311

## Description

The wheel smoke matrix on `ubuntu-22.04 / manylinux_x86_64` was targeting a stale PyPy build selector, causing cibuildwheel to reject the job before any wheel build began. This updates the workflow to use the currently supported PyPy identifier.

- **Wheel matrix updates**
  - Replace `pp310` with `pp311` in the full wheel matrix.
  - Replace the smoke-matrix PyPy entry with `pp311`.

- **Matrix exclusions**
  - Update PyPy-specific exclusion entries to match `pp311` so the matrix shape stays aligned with cibuildwheel’s current supported targets.

- **Result**
  - The workflow now selects a valid PyPy wheel target for Linux/macOS instead of the removed `pp310` selector.

```yaml
# before
python-version: pp310
CIBW_BUILD: pp310-manylinux_x86_64

# after
python-version: pp311
CIBW_BUILD: pp311-manylinux_x86_64
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a6e1c98f7ad0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.