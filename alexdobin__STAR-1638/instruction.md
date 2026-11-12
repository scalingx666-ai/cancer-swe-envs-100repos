# Issue

**Title:** Increased entropy of shmKey to avoid collisions between genomes.

## Description

I recently ran into an issue where trying to `--genomeLoad LoadAndKeep` a genome would stall with the error message `"Another job is still loading the genome, sleeping for 1 min"` despite there being no other such job.

It turns out that in some circumstances, loading a genome after a different genome has previously been loaded
can fail due to the different genome directories hashing to the same `shmKey`. When this kind of collision happens, attempting to load genome B after genome A will never make it out of the `while` loop below because the comparison will be between the loaded size of genome A and the expected size of genome B:

https://github.com/alexdobin/STAR/blob/ffb66fb04e2642d1c2df87a31344552487ec491d/source/Genome_genomeLoad.cpp#L219

In the current code, the shmKey for a file path is calculated using `ftok`: https://github.com/alexdobin/STAR/blob/51b64d4fafb7586459b8a61303e40beceeead8c0/source/Genome.cpp#L19

[This stackoverflow answer](https://stackoverflow.com/a/54492934) suggests (and my tests confirm) that `ftok` only uses the lower 16 bits of the inode of the file path. Apparently, for some filesystems, these lower 16 bits have much less than 16 bits of entropy, so that collisions between different index directories are relatively common. [Here](https://gist.github.com/jeffhussmann/b794d928ad1b46064033780cf2800a45) is a gist for testing this in your own filesystem, and the first lines of output when run on mine showing the problematic collision between `/data/indices/hg38/STAR` and `/data/indices/e_coli/STAR`:

```
$ python find_ftok_collisions.py /data
385876096:
        /data
        /data/indices/hg38
        /data/indices/e_coli
385876097:
        /data/indices/hg38/STAR
        /data/indices/e_coli/STAR
(more output truncated)
```

To prevent this issue, this pull request changes the `shmKey` calculation to just use an index directory's `st_ino` value directly instead of using `ftok` (which effectively means not throwing away any bits from `st_ino` but no longer using `st_dev` and `SHM_projectID` at all). This should ensure that different genome directories cannot collide.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e8a99c8892b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.