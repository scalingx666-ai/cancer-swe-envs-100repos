# Issue

**Title:** Prevent libcurl/libcrypto/et al being bundled in Linux wheels

## Description

An alternative to PR #1415 which reapplied the CA bundle location hack to _hfile_s3.c_. Because Linux wheels bundle _libcurl.so_ and are built on AlmaLinux (i.e., a Red Hat flavoured distro); and because Red Hat et al and Debian et al place their CA bundles in different locations, we need to explicitly set the Debian location when running on a Debian-flavoured platform. However this became more difficult with Fedora 44 — which uses a third location, different again from the existing two locations.

Instead dynamically load _libcurl_ so that the wheel repair machinery will not detect its use and bundle it. Then we will be using the native platform's _libcurl_ and won't need to correct the CA bundle location on non-RedHat platforms. Once we're doing that for _libcurl_, we might as well do it for _libcrypto_ too.

Avoiding bundling these networking and security-related libraries has other advantages too:

* 
* 
*  once and for all
* Avoids _libcurl_, _libcrypto_, and the twenty (20) other libraries they require (on manylinux_2_28) being pulled into the final wheel. This reduces Linux wheel size by ~4 MB.

Linux wheels will still bundle _libbz2_ and _liblzma_, but these are relatively small and hopefully do not have the same FIPS implications as these other cryptography-related libraries.

The trade-off is that pysam is now responsible for ensuring it can find these libraries successfully itself. This is relatively easy for _libcurl.so.4_ which has had the same soversion for a very long time and IIRC does not expect to bump it.

It is harder for _libcrypto.so.N_. Extant soversions are `1.1` (which is the baseline that is currently bundled in manylinux_2_28 wheels) and `3` (which is common on current platforms and is the musllinux_1_2 baseline). OpenSSL 4 has relatively recently been released, and Fedora 45 will use _libcrypto.so._`4`.

So we probe for all three _libcrypto_ soversions. We also add `pysam.set_runtime_libraries()` to enable the user to override the sonames to be loaded as a last resort, but (for now) don't particularly document it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dbcca7bec844`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.