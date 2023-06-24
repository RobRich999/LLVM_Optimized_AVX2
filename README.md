**LLVM_Optimized_AVX2:**

Clang/LLVM built for Ubuntu 23.04 and similar platforms using a modified Chromium build script. The build targets Linux x86-64-v3 with the Clang, LLD, Polly, and BOLT projects being built.

----

**Link to latest release build:**

https://github.com/RobRich999/LLVM_Optimized_AVX2/releases/tag/llvm-v17-r08be779-linux64-avx2

The binary builds included in this repository are accomplished under Kubuntu 23.04 (Lunar Lobster) via the modified build script and the following options:

vpython3 tools/clang/scripts/build.py --without-android --without-fuchsia --disable-asserts --thinlto --pgo --bolt --llvm-force-head-revision --x86-only --without-clang-extra --host-cc=/usr/lib/llvm-17/bin/clang --host-cxx=/usr/lib/llvm-17/bin/clang++ --gcc-toolchain=/usr

Similar to how a subset of LLVM binaries are packaged with Chromium checkouts, these binary builds include the basic tools needed for accomplishing a release build of the Chromium web browser.

----

**Script modifications and details:**

Apply the patch via the /chromium/src directory to modify the LLVM build script.

git apply /path/to/llvm-avx2.patch

All builds are bootstrapped. There is no need to pass the --bootstrap option, which will now error.

LLVM is built with -march=x86-64-v3 and other optimizations. Optimizations have been carried down into the PGO, ThinLTO, and BOLT build options.

Use --without-android and --without-fuchsia to skip downloading the ARM sysroots.

Use --x86-only to skip building LLVM support for various other architectures.

Use --without-clang-extra to disable building extra clang tools. These are not required for my Chromium release builds. YMMV for other build types.

Conditionals have been included for other architectures and LLVM projects, but they are unsupported and remain untested at this time.

Usage example from the /chromium/src directory:

vpython3 tools/clang/scripts/build.py --without-android --without-fuchsia --disable-asserts --thinlto --pgo --bolt --llvm-force-head-revision --x86-only --without-clang-extra

Building LLVM with ThinLTO, PGO, and BOLT optimizations are optional. Regardless, LLLVM still builds with optimizations for -O3, -march=x86-64-v3, Polly, etc.

PGO and BOLT are not too LLVM build time intensive for a relatively fast system and/or lots of cores. ThinLTO can incur dramatically increased LLVM build times.

****

**Note regarding mimalloc:**

Note a local build of the mimalloc allocator is pulled into the LLVM build script as a replacement for the standard Linux malloc allocator. Those using the modified script will need to obtain and build mimalloc, then update the path to libmimalloc.a calls in the script accordingly. Otherwise libmimalloc.a can be removed from the linker calls if malloc is preferred.

https://github.com/microsoft/mimalloc

Future script modifications are planned to enable or disable mimalloc integration via a build-time conditional option.

****

<sub>*Typical third-party build disclaimer. No warranties. No guarantees. Your mileage may vary. Use at your own risk.*</sub>
