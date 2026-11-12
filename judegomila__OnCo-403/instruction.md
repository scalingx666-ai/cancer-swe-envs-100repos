# Issue

**Title:** Bot proposals: rows that need a person

## Description

Drafted by `scripts/propose-updates.ts` from `public/factcheck.json`, `public/trials/changes.json`, `public/fda/recent.json` and `public/regional/candidates.json`, regenerated against today's main.

Review `docs/proposals/latest.md`: each line names the entity, the file and field, the current and proposed values, the source URL and, for EMA rows, why a person is needed (no exact product match, biosimilar or generic, a register status with no helper, a pending opinion, or a hand-written row that disagrees). EU rows the register decided on its own were merged by the auto branch of the same run and are listed at the end of the file, not proposed.

Apply the ones that check out by editing the data files (or push commits to this branch), then merge; close the rest. `CORRECTIONS.md` carries one line for this run under "Proposed by the update bot"; move confirmed corrections into the dated tables above it.

Merged automatically when it carries nothing but the regenerated report; held for a person as soon as it touches anything else.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ba4e9bc582d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.