**LLVM_Optimized_AVX2:**

Clang/LLVM built for Ubuntu 23.10 and similar platforms using a modified Chromium build script. The build targets Linux x86-64-v3 with the Clang, LLD, Polly, and BOLT projects being built.

----

**Link to latest release build:**

https://github.com/RobRich999/LLVM_Optimized_AVX2/releases/tag/llvm-v18-rbde5717-linux64-avx2

The binary builds included in this repository are accomplished under Uubuntu Server 23.10 (Mantic Minotaur) via the modified build script and the following options:

python3 tools/clang/scripts/build.py --bootstrap --without-android --without-fuchsia --disable-asserts --thinlto --pgo --bolt --llvm-force-head-revision

Similar to how a subset of LLVM binaries are packaged with Chromium checkouts, these binary builds include the basic tools needed for accomplishing a release build of the Chromium web browser.

----

**Script modifications and details:**

Apply the patch via the /chromium/src directory to modify the LLVM build script.

git apply /path/to/llvm-avx2.patch

LLVM is built with -march=x86-64-v3 and other optimizations. Optimizations have been carried down into the PGO, ThinLTO, and BOLT build options.

Usage example from the /chromium/src directory:

vpython3 tools/clang/scripts/build.py --bootstrap --without-android --without-fuchsia --disable-asserts --thinlto --pgo --bolt --llvm-force-head-revision

Building LLVM with ThinLTO, PGO, and BOLT optimizations are optional. Regardless, LLLVM still builds with optimizations for -O3, AVX2, etc.

PGO and BOLT tend to not be too LLVM build time intensive for a relatively fast system and/or lots of cores. ThinLTO can incur dramatically increased LLVM build times.

****

**Note regarding mimalloc:**

The mimalloc allocator is automatically pulled into the LLVM build script as a replacement for the standard Linux malloc allocator.

https://github.com/microsoft/mimalloc

****

**Note regarding Windows cross-building:**

The release builds include the needed compiler-rt libraries to support the Linux cross-building of Chromium for Windows.

Alternatively, if using the build script, the LLVM lib/clang/18/lib/windows/ library files will need to be copied manually from the LLVM package bundled by Chromium.

****

<sub>*Typical third-party build disclaimer. No warranties. No guarantees. Your mileage may vary. Use at your own risk.*</sub>
