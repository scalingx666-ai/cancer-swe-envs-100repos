# Issue

**Title:** Restore MHCflurry's download cache after isolated-home artifact tests (4.9.3)

## Description

Fixes 12 `tests/test_mhcflurry.py` failures in the full release gate (`TEST_SH_MAX=2 ./test.sh --require-all`). They reproduce on master 8b67d06 (4.9.0) and are unrelated to #628 or #630.

**Cause.** MHCflurry 2.3.25 resolves its download directory from `HOME` on first use and caches it for the process; 2.3.16 resolved it at import. `tests/test_artifacts.py`'s `no_user_installs` fixture points `HOME` at a temporary directory. When `test_list_includes_native_and_packaged_artifacts` is the first MHCflurry lookup on an xdist worker, that temporary home stays cached, and every later MHCflurry test on the worker fails with `Missing MHCflurry downloadable file`. Running those two tests on one worker reproduces it every time.

**Fix.** The fixture snapshots `mhcflurry.downloads._DOWNLOADS_DIR` and `_CURRENT_RELEASE` with `monkeypatch.setattr`, so they are restored after the test. MHCflurry's public `configure()` can't do this from the fixture, because pytest undoes the `HOME` change only after this fixture's teardown. Test-only change; no library code changes.

**Chain.** Based on #630 (4.9.2), which contains #628 (4.9.1). Merge order: #628 → #630 → this (4.9.3); release 4.9.3.

**Verified locally** on #630 plus this fix, with every backend provisioned (`MHCTOOLS_TEST_ENV=…/env/test-backends/activate.sh`): `./lint.sh` clean; `TEST_SH_MAX=2 ./test.sh --require-all -ra`: 2402 passed, 0 failed, 0 skipped. Without the fix: #630 gives 2389 passed and 12 failed; master gives 2381 passed and the same 12 failed.

https://claude.ai/code/session_01JHJhwj29oDqoeu7TYd34Pi

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2d9268c2c97e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.