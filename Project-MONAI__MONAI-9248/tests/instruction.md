# Issue

**Title:** Fix compiled lazy resampling with align_corners=True

## Description

Related to #9112.

### Description

Fixes lazy resampling with the compiled extension (`BUILD_MONAI=1`) when `align_corners=True`, following @ericspod's confirmation that the native `grid_sample` path is the reference.

In `spatial_resample`, the compiled branch builds `Affine(affine=xform, normalized=True, align_corners=True)` from a matrix that is already in voxel space. `AffineGrid` and the compiled `Resample` then apply the `align_corners=True` scalings (`d_out/(d_out-1)` and `(d_in-1)/d_in`) on top of it, distorting the grid passed to `grid_pull`. For non-integer `mode`, the matrix is now pre-compensated so those scalings cancel. Integer `mode` is unchanged.

Verification (`BUILD_MONAI=1` unless noted):
- `test_affine` + `test_affined`: 132 failed -> 0 failed.
- Wider set (spacing, resize, zoom, rotate, rand_* transforms): 10 failed -> 0 failed. Default (non-compiled) run: 0 failed, unchanged.
- Eager `Spacing` vs a plain `F.grid_sample(align_corners=True)` reference on three shapes: compiled was 0.2 to 0.5 off before, ~2e-7 after (native ~3e-8).
- Reproduces on CPU too, so not GPU-specific.

Test changes:
- Added `test_lazy_rotate_align_corners_compiled` (skipped unless the extension is compiled). It fails without the fix and passes with it.
- `_TEMPLATE_5_COMPILED` in `test_spacing.py` changed from 0.75 to 1.0. All sampled input coordinates for that case lie inside the image and the input is all ones, so 1.0 is the geometrically correct value. The native template (0.9553) looks like a unit-size-axis artifact and is untouched.

Note: `BUILD_MONAI=1` must be set at runtime, not only at build time, for these tests to exercise the compiled path. Tested locally on RTX 3050 / WSL2 / PyTorch 2.14.1+cu132 (outside CI's range); not run on PyTorch 2.8 to 2.11 or through the full `./runtests.sh`.

### Types of changes
<!--- Put an `x` in all the boxes that apply, and remove the not applicable items -->
- [x] Non-breaking change (fix or new feature that would not break existing functionality).
- [ ] Breaking change (fix or new feature that would cause existing functionality to change).
- [x] New tests added to cover the changes.
- [ ] Integration tests passed locally by running `./runtests.sh -f -u --net --coverage`.
- [ ] Quick tests passed locally by running `./runtests.sh --quick --unittests  --disttests`.
- [ ] In-line docstrings updated.
- [ ] Documentation updated, tested `make html` command in the `docs/` folder.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0e9fc83b1935`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.