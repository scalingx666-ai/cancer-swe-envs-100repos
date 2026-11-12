# Issue

**Title:** fix(security): clear the npm and pip-audit Security Scan failures

## Description

## Summary

Fixes the two Security Scan jobs that fail on main: "Frontend npm audit" and "Python dependency audit (pip-audit)". The frontend moves from Next.js 14 to 15.5.28 (React 19) and from Vitest 3 to 4. The API replaces python-jose with PyJWT in `api/routes/auth.py`. Nothing on the ranking path is touched.

## Problem

| Job | Failure on main |
|---|---|
| Frontend npm audit | `npm audit --audit-level=critical` reports 3 critical packages. `next`: every 14.x release is inside GHSA-p293-qw3h-jr36, GHSA-2xp9-vwfh-vxw4 and GHSA-2464-fhr4-w9xw, and no 14.x patch exists. `tinypool` (via `vitest` 3): GHSA-5gmw-xhrv-c9v3, GHSA-85c8-ppgw-ccpr. |
| pip-audit | `python-jose` 3.5.0 has PYSEC-2026-4298 with no fixed version. |

Next.js 15.5.28 (the `backport` dist-tag) is the smallest release outside every Next.js advisory range. The only other option is 16.x.

## Changes

**Frontend (`web/`)**

- `next` and `eslint-config-next` move to 15.5.28.
- The Next 15 App Router runs on React 19, so `react`/`react-dom` go to 19.3.1 with `@types/react`/`@types/react-dom` 19. Packages whose peer range stopped at React 18 are bumped: `@react-three/fiber` 9.8.2, `@react-three/drei` 10.7.9, `@stripe/react-stripe-js` 3.11.0, `lucide-react` 0.469.0.
- `vitest` and `@vitest/coverage-v8` go to 4.1.12.
- `npm audit fix` (without `--force`) applied the remaining non-breaking fixes. The lockfile was upgraded in stages, because a single in-place install hits ERESOLVE against the old React 18 tree.
- In Next 15, dynamic route `params` is a Promise. The four client pages that read it (`crowdfund/[id]`, `gene/[symbol]`, `repurposing/[id]`, `results/[id]`) now unwrap it with React's `use()`.
- `CLAUDE.md`, `README.md` and `docs/ARCHITECTURE.md` now say Next.js 15.

**API**

- `api/routes/auth.py` uses PyJWT: `jwt.get_unverified_header`, then `jwt.decode` with a `jwt.PyJWK` built from the selected JWKS entry. Both `except` clauses catch `jwt.PyJWTError`. The algorithm allow-list (RS256 only), issuer, audience, `exp`/`nbf` checks, the required `exp`, JWKS caching and the 401/503 responses stay the same.
- `api/requirements.txt`: `python-jose[cryptography]==3.5.0` is replaced by `PyJWT[crypto]==2.15.1`.
- `api/.pip-audit-ignore`: the ecdsa entry (PYSEC-2026-1325) is removed. ecdsa came in only through python-jose.
- `api/tests/test_auth_jwks.py` mints tokens with PyJWT and generates keys with `cryptography` directly. Three tests are new.

## Impact

PyJWT and python-jose differ in two places. Each is handled on purpose and covered by a test:

| Case | python-jose | Now |
|---|---|---|
| Audience configured, token has no `aud` claim | Accepted (the audience check returned early) | Rejected with 401. `config.py` requires `KEYCLOAK_AUDIENCE` in production, and Keycloak's audience mapper always sets `aud`, so valid tokens are unaffected. |
| `iat` slightly in the future (clock skew) | Accepted (only checked that `iat` is an integer) | Still accepted. PyJWT would reject it, so `verify_iat` is off and `exp`/`nbf` carry the validity window. |

On the frontend, the 3D hero (`components/hero/CaseCluster.tsx`) now runs on R3F 9. It renders in the production build with no console errors, only three.js deprecation warnings. The Next 15 build regenerates `web/next-env.d.ts` with a reference into `.next/`. That change is not committed, because the lint job runs `tsc --noEmit` without a build.

## Verification

| Check | Result |
|---|---|
| `npm audit --audit-level=critical` (web) | 0 critical. 8 high and 3 moderate remain (see below) |
| `pip-audit -r requirements.txt` with the trimmed ignore file | No known vulnerabilities found |
| `npx tsc --noEmit` | Pass |
| `npx eslint . --ext .ts,.tsx --max-warnings 0` | Pass |
| `npm run test:coverage` (Vitest 4) | 46/46 pass |
| `next build` (demo mode) | Pass |
| Playwright E2E (demo mode, `CI=1`) | 5/5 pass |
| Production server, manual check of `/`, `/results`, `/repurposing`, `/crowdfund`, `/gene` dynamic routes | Render without React errors |
| `pytest api/tests/test_auth_jwks.py` | 25/25 pass (22 existing, 3 new) |
| `ruff check` on the changed Python files | Pass |
| `PYTHONPATH=. pytest api/tests/` (3 RDKit modules skipped locally, they run in CI) | 1218 passed, 3 skipped, 2 xfailed |
| `PYTHONPATH=. pytest ai/tests/` | 42 passed |

## Not included

- The remaining high/moderate npm findings need breaking upgrades: `braces`/`micromatch` through `tailwindcss` 3 (fixed in tailwind 4) and the `postcss` copy bundled inside `next` (fixed in 16.x). Neither blocks the critical-level gate.
- `scripts/_write_readme.py` still writes "Next.js 14" into the README template. It sits under the protected `scripts/**` path and needs a human edit.
- The starlette entries in `api/.pip-audit-ignore` predate the FastAPI 0.141 bump and may now be stale. Left as is.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0bd8be141960`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.