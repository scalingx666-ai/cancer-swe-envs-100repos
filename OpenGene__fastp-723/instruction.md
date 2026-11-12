# Issue

**Title:** fix: scale reader backpressure with worker thread count

## Description

. Also  (same bug, reported independently against v1.3.5 — see comment below).

Also fixes the reproducible high-thread-count hang reported in #694 (confirmed by direct repro: mismatched-read-count input hangs at `-w 33`/`-w 48` on master, completes on this branch — see ). #694 additionally describes an intermittent hang at low thread counts on v1.3.3, predating this regression — that looks like a separate, narrower race in the mismatched-read-count/`shouldStopReading` shutdown path and is **not** fixed by this PR; tracking separately.

## Summary

With more than 32 worker threads (`-w 33` or higher, on a machine with at least 33 cores), fastp 1.3.x can hang forever right after startup / adapter detection. All reader and worker threads sit in `mBackpressureCV.wait_for`, no output is written, and CPU usage is near idle.

This PR makes the reader backpressure limits scale with the worker thread count, so readers always let each worker receive a pack it can consume. It does not touch `SingleProducerSingleConsumerList`, so the #695 fix (b402d71) stays as it is.

## Root cause

- Readers deal packs round-robin to one input list per worker, and pause once they are more than `PACK_IN_MEM_LIMIT` (32) packs ahead of processing (`peprocessor.cpp` `readerTask` / `interleavedReaderTask`, `seprocessor.cpp` `readerTask`).
- Since b402d71 (#695), an item in `SingleProducerSingleConsumerList` only becomes consumable once another item is produced behind it, or once the producer finishes. So each worker needs **two** packs in its list before it can start.
- With `T > PACK_IN_MEM_LIMIT` workers, the readers stop after 33 packs, before any list has a second item. Workers wait for input and readers wait for workers, so nothing makes progress.
  - 19602ae (v1.3.2) fixed exactly this case by marking the first item consumable.
  - b402d71 removed that line to fix #695, so the deadlock came back in 1.3.4 through 1.3.7.
- The reader-vs-writer check has the same shape. The writer drains worker buffer lists round-robin and waits on a list until its worker produces a second output. So `bufferLength() > PACK_IN_MEM_LIMIT` can also block the readers permanently when `T > 32`.
- fastp caps `-w` at `hardware_concurrency()`, so the hang only shows up on machines with at least 33 cores. That's probably why it looks machine-dependent in the reports.

Stack traces of a hung 1.3.6 process (`-w 48`, 48-vCPU VM; official binary, with symbols):

```
48 x PairEndProcessor::processorTask   src/peprocessor.cpp:1040   (condition_variable::wait_for)
 1 x PairEndProcessor::readerTask      src/peprocessor.cpp:825    (condition_variable::wait_for, left reader backpressure)
 1 x PairEndProcessor::readerTask      src/peprocessor.cpp:830    (condition_variable::wait_for, right reader backpressure)
 1 x main                              std::thread::join
```

## Fix

`packInMemLimit(threads) = max(PACK_IN_MEM_LIMIT, 2 * threads)` is used for both the reader/processor and the reader/writer backpressure checks. This applies to the PE reader, the interleaved-PE reader and the SE reader.

Memory stays bounded and grows only with the thread count: 2 packs of 1000 reads per worker. For example, at `-w 48` about 96k reads can be in flight per reader, up from 32k. At 16 threads or fewer the limits are unchanged; between 17 and 32 threads they grow as 2 × threads.

## Validation

Tested on a 48-vCPU AMD EPYC 7B13 VM running Ubuntu 24.04. Both builds came from source at 8a2397b, with and without this patch. Runs were killed if not finished after 120 s; normal runs take 3–16 s.

| Build | Data | Mode | Threads | Result |
|---|---|---|---|---|
| master | synthetic PE 500k pairs | PE + `--detect_adapter_for_pe` | 32 | OK 2/2 |
| master | synthetic | PE | 33 | **hang 3/3** |
| master | synthetic | PE | 48 | **hang 3/3** |
| master | SRR891268 (2M pairs) | PE | 48 | **hang 2/2** |
| master | synthetic | SE | 48 | **hang 2/2** |
| master | synthetic | interleaved PE | 48 | **hang** |
| official 1.3.6 | SRR891268 | PE | 48 | **hang 1/1** |
| this PR | synthetic | PE | 1 / 16 / 32 | OK 3/3 each |
| this PR | synthetic | PE | 33 / 40 / 48 | **OK 10/10 each** |
| this PR | SRR891268 | PE (with and without adapter detection) | 48 | OK 3/3 each |
| this PR | synthetic | SE, merge (`-m -c`), uncompressed output, `--stdout` | 1 and 48 | OK 3/3 each |
| this PR | synthetic | uncompressed output (#695 path) | 16 | OK 3/3 |
| this PR | synthetic | interleaved PE (`--interleaved_in`) | 1 / 48 | OK 1/1, 3/3 |

**Output equivalence:** for every mode and dataset, the sorted decompressed output records (md5 per output file) and the JSON after-filtering read/base counts are identical across this PR at `-w 1`, `-w 16` and `-w 48`, master at `-w 16`, and official 1.3.2 and 1.1.0.

**Performance** (SRR891268 subset, `-w 48`, median of 3): this PR 4.4 s, 1.3.2 4.0 s, 1.1.0 5.9 s.

**Follow-up thread-count sweep** (separate 48-vCPU run, larger 4M-pair SRR891268 subset, median of 3, `--detect_adapter_for_pe`):

threads | 1.1.0 | 1.3.2 | master | this PR
--- | --- | --- | --- | ---
1 | 38.9s | 30.8s | 31.9s | 31.2s
8 | 8.0s | 8.6s | 8.7s | 8.8s
16 | 8.6s | 7.7s | 8.1s | 8.1s
24 | 8.4s | 9.0s | 10.4s | **8.3s**
32 | 9.4s | 7.9s | **52.7s** | **8.8s**
33-48 | 9-10.5s | 7.8-8.5s | **HUNG** | 8.6-9.5s

At `-w <= 16` the new limit equals the old one, so timing is identical to master (as expected — no functional change in that range). At `-w 17-32`, `2*threads` exceeds the old fixed 32-pack cap, which incidentally reduces reader/writer stalling and measurably speeds things up — most visibly at exactly `-w 32`, where master doesn't fully deadlock but throttles to near-serial throughput (52.7s vs 8.8s, reproducible 3/3). So this change isn't neutral: unaffected at low thread counts, strictly faster in the 17-32 range, and turns a hang into a normal run above 32.

**ThreadSanitizer** (`-O1 -fsanitize=thread`, 200k-pair synthetic input, run under `setarch -R` because TSan aborts with "unexpected memory mapping" on high-entropy ASLR kernels): this PR adds no new reports.
- Master at `-w 16` and `-w 32` completes with the same set of warnings as this PR at `-w 16` / `-w 32` / `-w 33` / `-w 48`, all in existing code: `ReadPool::input` / `updateFullStatus` (`readpool.cpp:23/27/53`), `SingleProducerSingleConsumerList` `size()` / `produce()` / `consume()` (`singleproducersingleconsumerlist.h:90–121`), and `WriterThread::setInputCompletedPwrite` (`writerthread.cpp:82`).
- None are in the backpressure code touched here. Master can't be checked at `-w 33` or higher because it deadlocks.
- One consume-path line (`:121`) only shows up at 33+ threads, i.e. on the code path that the deadlock currently prevents from running.
- These look worth a separate look, but are out of scope for this PR.

## Reproduce

Any machine with at least 33 cores:

```bash
# public ATAC-seq run (Buenrostro et al. 2013), first 2M pairs
for i in 1 2; do
  curl -s https://ftp.sra.ebi.ac.uk/vol1/fastq/SRR891/SRR891268/SRR891268_$i.fastq.gz | gzip -dc | head -n 8000000 | gzip -1 > srr_R$i.fastq.gz
done
fastp -w 48 --detect_adapter_for_pe -i srr_R1.fastq.gz -I srr_R2.fastq.gz -o o1.fq.gz -O o2.fq.gz   # hangs on 1.3.6 and 1.3.7/master (tested); completes with this PR
fastp -w 32 --detect_adapter_for_pe -i srr_R1.fastq.gz -I srr_R2.fastq.gz -o o1.fq.gz -O o2.fq.gz   # completes on all versions
```

Single-end input also hangs at `-w 48` (reproduced with synthetic reads).

From the code, 1.3.0–1.3.1 (no first-item fix yet) and 1.3.4–1.3.5 (after b402d71) should behave the same; 1.3.2–1.3.3 include 19602ae (1.3.2 completed at `-w 48` in my tests; 1.3.3 wasn't tested). Workaround for affected releases: `-w 32` or lower.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a2397b6628a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.