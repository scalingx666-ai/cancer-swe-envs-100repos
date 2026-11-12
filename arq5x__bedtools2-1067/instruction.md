# Issue

**Title:** Fix various GCC 13 warnings and errors

## Description

As noted in #1056 et al, on recent platforms _ParseTools.cpp_/_.h_ fail to compile due to `uint32_t` not being declared. The obvious fix is to add `#include <cstdint>` but we can tidy things up a bit by fixing this differently. This PR fixes that build failure and also fixes several warnings encountered with GCC 13:

Due to improvements in system and/or compiler header cleanliness, where previously _ParseTools.{cpp,h}_'s headers have included `<cstdint>` as a byproduct, this no longer occurs. Hence:
 - Ordinary unsigned is appropriate for the int2str() variable anyway;
 - Get CHRPOS from BedtoolsTypes.h instead of typedeffing it ourselves;
 - ParseTools.cpp's isHeaderLine() does use uint32_t, so include `<cstdint>` from the .cpp file for good measure.

Other warnings:
- Assignment operators should not be virtual (avoids "Record::operator= was hidden" warnings).
- List member initialisers in the same order in which they are declared (avoids "_testLastQueryRec will be initialized after _runToDbEnd" warnings).

.  and .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `07533e9bce0d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.