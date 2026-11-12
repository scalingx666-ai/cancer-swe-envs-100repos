# Issue

**Title:** [CTM-684][CTM-683][CTM-682][CTM-696][CTM-697] Security dependency updates

## Description

### Description

Addressing a bunch of security tickets. The changes in the logback files are due to the logback version bump changing the syntax for conditionals. No behavior changes.

| Ticket | Advisory | Severity | Package | Before | After | How |
|---|---|---|---|---|---|---|
| [CTM-684](https://broadworkbench.atlassian.net/browse/CTM-684) | [CVE-2026-19880](https://github.com/advisories/GHSA-9mh8-hq67-v26g) | Medium | `ch.qos.logback:logback-{access,classic,core}` | 1.5.34 | 1.6.6 | `logbackV` bump |
| [CTM-683](https://broadworkbench.atlassian.net/browse/CTM-683) | [CVE-2026-73495](https://github.com/advisories/GHSA-46q4-43ph-c6fr) | High | `org.http4s:blaze-{core,http}` | 0.14.18 | 0.23.18 | `dependencyOverrides` (http4s 0.21 is EOL, no fixed release) |
| [CTM-682](https://broadworkbench.atlassian.net/browse/CTM-682) | [CVE-2026-59903](https://github.com/advisories/GHSA-8c42-7qj2-3j46) | Medium | `io.netty:netty-*` | 4.2.16.Final | 4.2.19.Final | `nettyV` bump (all overridden netty modules) |
| [CTM-696](https://broadworkbench.atlassian.net/browse/CTM-696) | [CVE-2026-107282](https://github.com/advisories/GHSA-jmqq-x5g9-9p2w) | Critical | `org.asynchttpclient:async-http-client` | 2.12.4 | 2.16.1 | existing override bumped |
| [CTM-697](https://broadworkbench.atlassian.net/browse/CTM-697) | [CVE-2026-106451](https://github.com/advisories/GHSA-mcr4-qmvw-px4g) | High | `at.yawk.lz4:lz4-java` | 1.11.0 | 1.11.4 | `lz4JavaV` bump |


[CTM-684]: https://broadworkbench.atlassian.net/browse/CTM-684?atlOrigin=eyJpIjoiNWRkNTljNzYxNjVmNDY3MDlhMDU5Y2ZhYzA5YTRkZjUiLCJwIjoiZ2l0aHViLWNvbS1KU1cifQ
[CTM-683]: https://broadworkbench.atlassian.net/browse/CTM-683?atlOrigin=eyJpIjoiNWRkNTljNzYxNjVmNDY3MDlhMDU5Y2ZhYzA5YTRkZjUiLCJwIjoiZ2l0aHViLWNvbS1KU1cifQ
[CTM-682]: https://broadworkbench.atlassian.net/browse/CTM-682?atlOrigin=eyJpIjoiNWRkNTljNzYxNjVmNDY3MDlhMDU5Y2ZhYzA5YTRkZjUiLCJwIjoiZ2l0aHViLWNvbS1KU1cifQ
[CTM-696]: https://broadworkbench.atlassian.net/browse/CTM-696?atlOrigin=eyJpIjoiNWRkNTljNzYxNjVmNDY3MDlhMDU5Y2ZhYzA5YTRkZjUiLCJwIjoiZ2l0aHViLWNvbS1KU1cifQ
[CTM-697]: https://broadworkbench.atlassian.net/browse/CTM-697?atlOrigin=eyJpIjoiNWRkNTljNzYxNjVmNDY3MDlhMDU5Y2ZhYzA5YTRkZjUiLCJwIjoiZ2l0aHViLWNvbS1KU1cifQ

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ed560d59cc8b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.