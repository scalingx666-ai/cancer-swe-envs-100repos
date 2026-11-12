# Issue

**Title:** chore: prepare 2.2.4 CRAN release (purrr defunct compatibility, #139)

## Description

## Summary

Prepares the next CRAN release (2.2.4) in response to #139 (@hadley): purrr's next release makes functions deprecated in purrr 1.0.0 defunct (including `when()`), and a new CRAN release was requested.

### Verification performed

- **master** uses no function deprecated in purrr 1.0.0: checked unqualified calls (NAMESPACE does `import(purrr)`) and qualified `purrr::` calls for `when()`, `invoke()`/`invoke_map()`, `splice()`, `update_list()`, `rerun()`, `prepend()`, `rdunif()`, `rbernoulli()`, `list_along()`, `cross()`/`cross2()`/`cross3()`/`cross_n()`, `lift_*`, `*_raw()` map variants, `reduce_right()`/`reduce2_right()`/`accumulate_right()`, `as_function()`, `is_numeric()`, and `%@%`. Only `purrr::map` is used — safe.
- **CRAN 2.2.3 tarball** (published 2026-05-30) contains the `when()` removal (PR #129, 2026-04-01) and is likewise clean of all the functions above, so the urgent breakage concern is already moot — but this release conclusively answers the request and ships the pending fixes below.

### Changes

- Bump version `2.2.3.9000` → `2.2.4` via `usethis::use_version("patch")`
- `NEWS.md`: fill the 2.2.4 release notes
- `cran-comments.md`: submission note referencing #139

### Fixes shipping in 2.2.4 (since 2.2.3)

- `download_iobr_data()`: internet check tries Baidu first (faster in China), each attempt capped at 5s
- `palettes()`: diverging palettes 2-5 now use 5 colors instead of 3
- `iobr_cor_plot()`: fix five-color heatmap palette handling
- `sig_heatmap()`: color scale anchors (-3, 3) → (-2, 2) to match displayed value clipping
- `surv_group()`: fix legend labels when no reference group is specified

## Test plan

- [x] `R CMD build . --no-build-vignettes` — builds `IOBR_2.2.4.tar.gz`
- [x] `R CMD check IOBR_2.2.4.tar.gz --no-manual` — **0 errors, 0 notes**; the 2 vignette WARNINGs are artifacts of `--no-build-vignettes` (missing `inst/doc`) and disappear in the full build used for CRAN submission
- [x] Examples pass (`checking examples ... OK`), data and `sysdata.rda` checks OK
- [ ] CI green on all three platforms

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `635effe4d05f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.