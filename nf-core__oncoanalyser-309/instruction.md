# Issue

**Title:** Remove vulnerable PR-comment artifact pattern (dev branch)

## Description

## Security patch

This PR applies a security fix included in **nf-core/tools 4.0.3** for a potential exploit in the GitHub Actions workflows that post comments on pull requests.

For details, see the related security advisory and discussion:

- https://nf-co.re/advisories/pr-comment-workflow-vulnerability
- https://nfcore.slack.com/archives/C04QR0T3G3H/p1784895743182849

These changes only affect CI workflows, so merging them into the default branch does **not** require a new pipeline release.

Recommendation from the linked Slack thread is to also merge the security patch into dev to ensure:

> "the changes will not get lost in a merge from dev to main/master"

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a2d25435924a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.