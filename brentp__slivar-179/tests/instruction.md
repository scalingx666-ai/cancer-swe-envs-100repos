# Issue

**Title:** Add CI

## Description

Re: 

Uses Homebrew instead of APT for Nim on Ubuntu since the [APT package](https://packages.ubuntu.com/noble/nim) uses Nim 1.6 on Ubuntu 24.04

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3155cda696b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.