# Issue

**Title:** Fix faidx output capture and improve pysam's use of -o for redirection

## Description

Capturing `faidx` output has been broken since v0.24.0 due to importing samtools ≥1.22 which newly uses `bgzf_open()` to write to standard output, similarly to the `calmd` command. Fix this by patching `bgzf_open("-", …)` and using the same `MAP_STDOUT_OPTIONS` technique as for `calmd`. Once available in a pysam release, this will provide a fix for sanger-pathogens/ariba#349.

If the user-specified arguments use `-o FILE` themselves, the caller needs to use `catch_stdout=False` to avoid the synthetic `-o` option used to capture stdout clobbering the desired `-o` option. Make this more robust by prepending the synthetic `-o` option rather than appending it, so that the user's option clobbers the synthetic one instead. .

(In future we aim to replace all this code with a more traditional file descriptor redirection approach, which will be simpler and more robust.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `93a9bf139f85`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.