# Issue

**Title:** Add console output to preview comments

## Description

- [x] This comment contains a description of changes (with reason)

## Description

This PR adds the MultiQC command and console output to preview comments posted on pull requests. This helps developers understand what command was run and see any warnings or errors that occurred during report generation.

### Changes

**scripts/post_preview_reports.py:**
- Added `CONSOLE_MAX` constant (50000 chars) to limit console output size in GitHub comments (which have a 65536 character cap)
- Extracted artifact download logic into a new `download()` function to avoid duplication
- Added `console_details()` function that downloads the console log artifact, truncates if needed, and returns a collapsible HTML details block with syntax highlighting
- Updated all preview comment posts to include console output via the new `console` variable

**.github/workflows/module-report-build.yml:**
- Modified the MultiQC run step to capture both the command and console output to a log file
- Added `set -o pipefail` to ensure the step fails if MultiQC fails
- Prefixed the log file with the actual command being run for clarity
- Added a new "Upload console log" step that uploads the log artifact on both success and failure (so comments can show why MultiQC failed)

### Rationale

When a preview report build fails or produces unexpected results, developers currently have no visibility into what went wrong without manually checking the workflow run logs. By including the console output in the preview comment itself, we provide immediate context and reduce friction in the development workflow.

The console output is truncated to keep the last 50KB (where errors typically appear) to avoid hitting GitHub's comment size limit, and is wrapped in a collapsible details block to keep comments concise.

## Test Plan

The changes are covered by existing CI workflows. The preview comment generation is tested by the actual workflow runs on pull requests.

https://claude.ai/code/session_01WjFr3KzDe6fdBzvri7Vwje

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6946a9c36d7a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.