# Issue

**Title:** Add ARM Neon and scalar implementations of SIMD functions

## Description

Building BWA has long been limited to x86-64 due to the unconditional usage of SSE2 SIMD intrinsics in _ksw.c_'s `ksw_u8()` and `ksw_i16()` functions. This PR adds Neon implementations of these functions, allowing compilation on Aarch64 machines, and also implementations using ordinary scalar C code that at least provide a working (albeit inefficient) build on other (little endian, at least) platforms.

This is done without duplicating the code in _ksw.c_: in most cases, by defining SSE2 intrinsics as `static inline` functions calling Neon intrinsics where there is a direct equivalent; in other cases, by analysing what the SSE2 code is doing semantically and using a non-directly-equivalent Neon intrinsic that has the same effect in the context of the code. (Similarly with the scalar implementation using a small inline C function for each SSE2 intrinsic.)

There are already BWA PRs #283 and #344, which port the SIMD code to other platforms using [SIMDe](https://github.com/simd-everywhere/simde) and [sse2neon](https://github.com/DLTcollab/sse2neon) respectively. The approach used in this PR is superior for several reasons:

1. It adds ~200 lines of code, rather than a ~800,000-line submodule (SIMDe) or an external ~9000-line header file (sse2neon).

2. The sse2neon approach adds support for ARM platforms only; this approach (and the SIMDe approach) add support for other platforms too — little endian platforms anyway; the code may need further porting to other endiannesses.

    (This is mostly of interest to distribution packagers who wish to have at least minimal support for other platforms; I am unaware of substantial communities of even potential BWA users on platforms other than x86 and ARM.)

3. This approach produces a more efficient ARM implementation than naive use of a direct SSE2 intrinsic emulation layer:

    * The `__max_16`/`__max_8` macros return the maximum entry in their vector argument.
 
       SSE2 does not implement this operation directly, so they are coded in terms of other SSE2 intrinsics. The `_mm_srli_si128` operation used does not have a direct equivalent in Neon; sse2neon implements it using a sequence of three Neon intrinsics involving memory I/O. Implemented thus, `__max_8` requires 3*(1+3)+1 = 13 Neon operations.

      However by understanding the semantics of the code (instead of simply translating individual operations), the entire `__max_8` computation can be done using a single Neon `vmaxvq_s16` intrinsic (involving only registers).

    * Similarly sse2neon implements `__mm_movemask_epi8` using a sequence of six Neon intrinsics. However analysing the code semantics (see `allzero_16` and `allzero_0f_8` below) shows that an identical effect can be achieved using a single `vmaxvq_u8`/`vmaxvq_u16` Neon intrinsic.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b56db225480b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.