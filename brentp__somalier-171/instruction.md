# Issue

**Title:** contamination: use the golden-ratio conjugate in the refinement search

## Description

I found that `golden_section_max` uses `goldenRatioConjugate = 0.3819660112501051`, which is 1/φ² rather than the conjugate 1/φ = 0.618.

With ~0.382, `c = hi - 0.382 * (hi - lo)` is the upper interior point and `d` the lower, but the update branches treat `c` as the lower one, so every iteration discards the side that is holding the maximum. The refined likelihood will then loses to the coarse grid point and `estimate_pair_contamination_from_sites` keeps the grid value, so `contamination -p` estimates sit on the coarse grid (0.01, 0.02, 0.05, ...) or on a first interior point such as 0.0076 (0.382 × 0.02).

Setting the constant to `0.6180339887498949` makes the branches the textbook golden-section search. In my hands and on real tumor-normal pairs from a targeted panel the estimates now agree with a 0.0005-step scan of `pair_total_log_likelihood`; before the fix, contamination near 4.5% read exactly 0.05.

The new test has 100 hom-ref anchor sites whose receiver carries 24 alt reads in 1,000. A 0.001-step scan of the likelihood peaks at 0.044; the current search returns 0.050 and fails the test, and the fix returns 0.0441.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1ff92a7af420`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.