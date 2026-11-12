# Issue

**Title:** 解决getmrnaexpr时TCGAbiolinks::GDCquery方法的workflow.type参数报错问题

## Description

作者您好，非常感谢您开发了如此方便的R包
我在按教程安装清华镜像源的最新的TCGAbiolinks依赖后，getmrnaexpr方法会报错，按照报错信息排查发现TCGAbiolinks::GDCquery方法的workflow.type参数应从"STAR - count"改为"STAR - Counts"，推测可能与TCGAbiolinks版本不同有关，更改后getmrnaexpr方法正常运行

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `71020ef7a88d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.