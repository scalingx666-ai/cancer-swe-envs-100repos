# Issue

**Title:** Native ARM/aarch64 build support (NEON via sse2neon)

## Description

## Summary

HISAT2 currently only builds on x86: it hardcodes SSE2 intrinsics and x86-only compiler flags, so a clean checkout fails to compile on Linux arm64 and macOS arm64 (Apple Silicon), typically dying with:

```
aligner_sw.h:73:10: fatal error: emmintrin.h: No such file or directory
```

This PR makes the source build **unpatched** on both `x86_64` and `aarch64`, on Linux and macOS, with **no behavioral change on x86**. It folds in the same set of edits that downstream packagers (e.g. Homebrew) currently carry as build-time patches.

## Changes

**Architecture gating (Makefile + CMakeLists kept in sync)**
- **SSE2 → NEON on ARM.** `aligner_sw.h` and `sse_util.h` now select `"sse2neon.h"` on ARM (`__aarch64__ || __arm__ || _M_ARM64`) and keep `<emmintrin.h>` on x86.
- **`sse2neon.h` vendored** under `third_party/` (DLTcollab/sse2neon, **v1.9.1**, MIT), so the build needs no system-installed copy. Provenance/license recorded in `third_party/README.md`.
- **`-msse2` gated to x86** in both `Makefile` (`SSE_FLAG`) and `CMakeLists.txt`; empty on ARM (invalid there).
- **`-DPOPCNT_CAPABILITY` gated to x86** in both build files. The existing non-POPCNT generic popcount path (`pop64`/`pop6464` in `gfm.h`) is used on ARM.
- **cpuid made arch-aware** in `processor_support.h`: the `<cpuid.h>` / `__get_cpuid()` path is restricted to `__amd64__ || __i386__`, and unsupported architectures return `false` gracefully instead of `assert(0)`.
- **`third_party` include gated**: ARM adds it via `-iquote third_party` (for the quoted `sse2neon.h` only, so the x86 `<cpuid.h>` is not exposed); x86 keeps `-I third_party` for `<cpuid.h>`.

**macOS `VERSION` vs `<version>`**
- The `VERSION` file plus `-I.` shadowed libc++'s standard `<version>` header on case-insensitive filesystems (`./VERSION:1:1: error: expected unqualified-id`). Fixed by using `-iquote .` instead of `-I.` (Makefile, the `hisat2-inspect` recipes, and `add_compile_options(-iquote …)` in CMake). All local headers are quoted-included, so quoted-include search is sufficient. The `VERSION` file and the reported version string are unchanged (binaries still report `2.2.2`).

**64-bit detection on ARM (found while building)**
- The Makefile's `BITS` detection only recognized `x86_64`, so `uname -m = arm64`/`aarch64` defaulted to `BITS=32` and passed the x86-only `-m32` flag (which forces a 32-bit soft-float compile and breaks NEON on Apple Silicon). ARM now emits no `-m32`/`-m64`; the x86 path is untouched.

## No x86 regression

On x86 the flags are unchanged:
- `make -n hisat2-align-s ARCH=x86_64` still emits `-msse2`, `-DPOPCNT_CAPABILITY`, `-I third_party`, and the `BITS`/`-m64` block is untouched.
- The CMake arch regex routes `x86_64`/`amd64` to the x86 branch (`-m64 -msse2`, `-DPOPCNT_CAPABILITY`, `-I third_party`).
- `processor_support.h` compiles the identical cpuid path on x86.

## Test matrix

| Target | Status |
|---|---|
| macOS arm64 (Apple Silicon) | ✅ built + smoke-tested natively (this PR's dev host) |
| Linux aarch64 | ⚙️ covered by the same arch gating; not compiled by author |
| Linux x86_64 | ⚙️ flags verified unchanged via `make -n` / CMake configure; not recompiled |
| macOS x86_64 | ⚙️ same include fix as arm64; not compiled by author |

**Functional smoke test on macOS arm64** (clean `make`, all 7 binaries, 0 errors):

```
$ ./hisat2-build example/reference/22_20-21M.fa idx   # -> idx.1.ht2 produced
$ ./hisat2 -f -x idx -U example/reads/reads_1.fa -S out.sam
1000 reads; of these:
  1000 (100.00%) were unpaired; of these:
    0 (0.00%) aligned 0 times
    1000 (100.00%) aligned exactly 1 time
    0 (0.00%) aligned >1 times
100.00% overall alignment rate
```

## Notes
- Diff kept minimal and portable across GCC and Clang; no C++ standard bump beyond the existing `-std=c++11`.
- The large line count is entirely the vendored single-header `third_party/sse2neon.h`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `99583d7536b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.