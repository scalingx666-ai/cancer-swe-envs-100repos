# Issue

**Title:** Three China approvals cite the makers' own announcements

## Description

Data only. Three China (NMPA) approval rows now cite each company's own exchange disclosure, read as raw text, and say "approval reported by the company; regulator notice not read". nmpa.gov.cn blocks scripts and was not read, by any route.

Files: `src/data/regional-approvals.ts` (CN rows), plus the matching `approvals` entries in `src/data/drugs.ts`, `src/data/drugs-pipeline-wave1.ts` and `src/data/company-pipeline-gap.ts`.

## Iza-bren (izalontamab brengitecan, BL-B01D1), Sichuan Biokin, SSE 688506

- **Before:** approved 2026. Source was the SystImmune newsroom (US subsidiary). NPC indication was given as "after platinum chemotherapy and PD-1 or PD-L1 therapy".
- **Sources:** SSE announcement 2026-048, dated 23 Jun 2026, https://static.cninfo.com.cn/finalpage/2026-06-23/1225380568.PDF, and 2026-053, dated 17 Jul 2026, https://static.cninfo.com.cn/finalpage/2026-07-17/1225428812.PDF. Both were read through CNINFO.
- **What they say:** for each indication, the company received the NMPA drug registration certificate (药品注册证书). Neither announcement says the approval is conditional (附条件) or full. Approved indications, in English:
  - NPC: recurrent or metastatic nasopharyngeal carcinoma after **at least two lines of systemic chemotherapy** and a PD-1/PD-L1 inhibitor.
  - ESCC: adults with recurrent or metastatic oesophageal squamous cell carcinoma after a PD-1/PD-L1 inhibitor with platinum chemotherapy.
- **After:** status stays approved, because nothing read supports "conditional". The row says the disclosures do not state the approval type. The NPC indication is corrected to the line of therapy the company states.

## Trastuzumab botidotin (A166), Kelun-Biotech, HKEX 6990

- **Before:** approved 2025. It already cited the HKEX announcement.
- **Source:** HKEX voluntary announcement dated 17 Oct 2025, https://www1.hkexnews.hk/listedco/listconews/sehk/2025/1017/2025101700358.pdf
- **What it says:** "was approved for marketing by the National Medical Products Administration (NMPA) for adult patients with unresectable or metastatic HER2 positive breast cancer (BC) who have received one or more prior anti-HER2 therapy". It does not call the approval conditional and gives no approval date.
- **After:** approved 2025. The note is reworded to the company-reported form, quoting the announcement verbatim.

## Deutenzalutamide (HC-1119), Hinova Pharmaceuticals, SSE 688302

- **Before:** approved 2025 for "metastatic castration-resistant prostate cancer". Source was a Hinova pipeline page giving 29 May 2025. That page no longer showed the date when fetched on 11 Nov 2026.
- **Source:** SSE announcement 2025-030, dated 30 May 2025, https://static.cninfo.com.cn/finalpage/2025-05-30/1223719909.PDF (via CNINFO).
- **What it says:** the company learnt from the NMPA website that the NMPA had recently (近日) approved it. The announcement does not mention a conditional approval and gives no exact approval date. Approved indication, in English: adults with mCRPC that progressed after abiraterone acetate and chemotherapy, with no earlier novel androgen receptor inhibitor.
- **After:** the indication is narrowed to that wording. The unsupported "29 May 2025" date is removed. The tldr and summary now match.

## Gates

`npm run validate`, `typecheck` and `lint` pass (0 errors). `npx vitest run` passes on 9 files and 79 tests: regional-approvals, approval-chip, universe-regulatory, regional-approvals-io, dated-fields, corpus-rules, tone, audit and health-denominators.

Not merged, not deployed.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b1d278e27707`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.