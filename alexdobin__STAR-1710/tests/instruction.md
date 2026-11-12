# Issue

**Title:** standardise parameter format

## Description

This PR makes minor parameters to the parametersDefault file. It assumes the following format:

```
### <Argument group>
<Argument name>       <default>
    <type>: <short description>
        <long description>
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ec8b7e2edc5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.