# Issue

**Title:** Initial upgrade of unit tests to match cbio main junit version

## Description

- Removed the Surefire 2.21.0 pin, so the importer uses the Surefire version Spring Boot manages (3.5.6).
- Added the JUnit Vintage engine (6.0.3), which runs the JUnit 4 tests without rewriting them.
- Removed the cbioportal.version (v6.4.1) property.

The importer’s test run now takes about 3¼ minutes instead of 61, and every test still passes: 115 unit and 200 integration tests (3 skipped). I also ran the suite twice with test classes in random order, and both runs passed.

What made it slow: resetting inside one container, as you suggested, wasn’t enough by itself.

- Container restarts: a new ClickHouse container after every test method. Rebuilding the database inside the running container instead (drop, schema, seed) takes under a second.
- A new JVM for every test class: each one paid for a container start plus the ClickHouse driver’s start-up. The driver looks up a hostname for every network address on the machine, which took 40 seconds per class on this Mac. CI’s Linux runners probably don’t have that delay, so the CI saving may be smaller.

Changes

- IntegrationTestBase: after each test, it drops the test database and rebuilds it inside the same container. Dropping the whole database also removes the tables the importer creates while running (backup tables, scratch tables, the sequence table), so each test starts from the same state a new container gave it.
- importer/pom.xml: all integration test classes now run in one JVM, sharing one container.
- Leaks between classes: sharing one JVM made four tests fail, because state now carried over from earlier classes:
  - Six test classes turn the importer’s bulk-load mode on and never turn it off.
  - The gene, gene panel and reference genome caches still held data from before the database was rebuilt.

IntegrationTestBase now resets all of that before each test. That needed small public reset or reload methods in four importer classes:

- ClickHouseBulkLoader.reset()
- ClickHouseBulkDeleter.reset()
- DaoGenePanel.reCache()
- DaoReferenceGenome.reCache(), which was private before


The importer itself doesn’t call any of them. DaoGenePanel’s cache starts out the same way as before.

importer-maven.yml: the timeout drops from 120 to 30 minutes.
AGENTS.md: gives the new run time, and says that any new static state in the importer must also be reset in IntegrationTestBase.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d13e05b3d8b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.