# Issue

**Title:** master merge 2026-11-05 [VS-2057]

## Description

Merges `master` into `ah_var_store`. This brings in the two master commits since the last sync (VS-2026, 2026-09-24):

- ce377aeec (#9471): runs the M2 WDL tests' Cromwell with the Java that setup-java installs. `ubuntu-latest` runners are now ubuntu-26.04, whose default Java is 25. Under it Cromwell's Liquibase fails and the "WDL test RUN_M2_WDL on cromwell" job hangs until the 6-hour limit, which is currently failing CI on `ah_var_store` PRs (for example #9454).
- aa9a8076c (#8639): LearnReadOrientationModel loads tables for one ref context at a time, reducing memory 32x.

**Conflicts:** earlier syncs were squash-merged, so git still merges from a 2023 merge base and re-raised 7 conflicts in files where `ah_var_store` has long-standing, intentional divergences from master: `.gitignore`, `build.gradle` (parquet/pgen dependencies, `scripts/variantstore` WDL validation), `build_docker.sh` (`--iidfile`), `build_docker_remote.sh`, `IOUtils.java` (tar path-traversal guard), `GATKVCFConstants.java` (`SCORE`/`CALIBRATION_SENSITIVITY` naming), and `MockVcfWriter.java` (`getEmitted()`). None involve the two new commits. All were resolved to `ah_var_store`'s side.

**Verification:** the merged tree is identical to `ah_var_store` with ce377aeec and aa9a8076c cherry-picked on top, plus the GATK Docker image update in `GvsUtils.wdl` (7cb0ec723), so this PR changes exactly what those two commits change, and the image tags.

**Merge method:** pending the GATK maintainers' OK, we plan to land this as a true merge commit by briefly enabling "Allow merge commits" for this one merge. That makes `master` an ancestor of `ah_var_store`, so the next sync starts from this merge instead of the 2023 merge base and won't re-raise these conflicts. If it is squash-merged instead, the merge base stays at 2023 and the next sync will raise the same conflicts.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78cfb052975d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.