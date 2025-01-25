**LLVM_Optimized_AVX2:**

Clang/LLVM built for Ubuntu 24.04 and similar platforms using a modified Chromium build script. The build targets Linux x86-64-v3 with the Clang, LLD, Polly, and BOLT projects being built.

----

**Script modifications and details:**

Apply the patch via the /chromium/src directory to modify the Chromium project LLVM build script.

> git apply /path/to/llvm-avx2.patch

LLVM is built with -march=x86-64-v3 and other optimizations. Optimizations have been carried down into the PGO, ThinLTO, and BOLT build options.

Usage example from the /chromium/src directory:

> python3 tools/clang/scripts/build.py --bootstrap --without-android --without-fuchsia --disable-asserts --thinlto --pgo --bolt --llvm-force-head-revision

Building LLVM with ThinLTO, PGO, and BOLT optimizations are optional. Regardless, LLLVM still builds with optimizations for -O3, AVX2, etc.

PGO and BOLT tend to not be too LLVM build time intensive for a relatively fast system and/or lots of cores. ThinLTO can incur dramatically increased LLVM build times.

****

**Note regarding Windows cross-building:**

If cross-building Windows binaries, the LLVM lib/clang/20/lib/windows library files will need to be copied manually from the LLVM package bundled by Chromium.

****

<sub>*Typical third-party build disclaimer. No warranties. No guarantees. Your mileage may vary. Use at your own risk.*</sub>
