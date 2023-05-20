**LLVM_Optimized_AVX2:**

Clang/LLVM built for Ubuntu and similar platforms using a modified Chromium build script. The build targets Linux x86-64-v3 with the Clang, LLD, Polly, and BOLT projects being built. Conditionals have been included for other architectures and LLVM projects, but they are unsupported and remain untested at this time.

Apply the patch via the /chromium/src directory to modify the LLVM build script.

git apply /path/to/llvm.patch

All builds are bootstrapped. There is no need to pass the --bootstrap option, which will now error.

LLVM is built with -march=x86-64-v3 and other optimizations. Optimizations have been carried down into the PGO, ThinLTO, and BOLT build options.

Use --without-android and --without-fuchsia to skip downloading the ARM sysroots.

Use --x86-only to skip building LLVM support for various other architectures.

Use --without-clang-extra to disable building extra clang tools. These are not required for my Chromium release builds. YMMV for other build types.

Usage example from the /chromium/src directory:

vpython3 tools/clang/scripts/build.py --without-android --without-fuchsia --disable-asserts --thinlto --pgo --bolt --llvm-force-head-revision --x86-only --without-clang-extra

Building LLVM with ThinLTO, PGO, and BOLT optimizations are optional. Regardless, LLLVM still builds with optimizations for -O3, -march=x86-64-v3, Polly, etc.

PGO and BOLT are not too LLVM build time intensive for a relatively fast system and/or lots of cores. ThinLTO can incur dramatically increased LLVM build times.

****

**Note regarding mimalloc:**

Note a local build of the mimalloc allocator is pulled into the LLVM build script as a replacement for the standard Linux malloc allocator. Those using the modified script will need to obtain and build mimalloc, then update the path to libmimalloc.a in the script accordingly. Otherwise libmimalloc.a can be removed the linker directives if preferred.

https://github.com/microsoft/mimalloc

----

**Note regarding repo binary builds:**

Binary builds included in this repository are accomplished under Kubuntu 23.04 (Lunar Lobster) via the modified build script and the following options:

vpython3 tools/clang/scripts/build.py --without-android --without-fuchsia --disable-asserts --thinlto --pgo --bolt --llvm-force-head-revision --x86-only --without-clang-extra --host-cc=/usr/lib/llvm-17/bin/clang --host-cxx=/usr/lib/llvm-17/bin/clang++ --gcc-toolchain=/usr

****

<sub>*Typical third-party build disclaimer. No warranties. No guarantees. Your mileage may vary. Use at your own risk.*</sub>
