# Issue

**Title:** Adding functionality to annotate clones by driver mutations

## Description

![image](https://user-images.githubusercontent.com/13375875/49561953-7b072600-f8e7-11e8-8461-f62c480f6ba9.png)

![image](https://user-images.githubusercontent.com/13375875/49562133-3c25a000-f8e8-11e8-9ab7-d25310ba0b05.png)

Controlled by `clone.annots` optional parameter in createFishObject. Omit the option to not annotate anything, which is the default behavior

```
fish = createFishObject(
  frac.table,
  parents,
  timepoints=timepoints,
  clone.annots=c("DNMT3A,FLT3", "NPM1", "MET", "ETV6,WNK1-WAC,\nMYO18B")
)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d25f71f9191`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.