GNUstep Android Toolchain
=========================

This project comprises a collection of scripts to build a GNUstep toolchain for Android. The toolchain can then be used in an Android project to compile and run Objective-C code using the Foundation and CoreFoundation libraries.

The toolchain is built using the compiler and tools provided by the standard Android SDK and NDK (installed e.g. via [Android Studio](https://developer.android.com/studio)). It is currently set up to target Android API level 21 (5.0 / Lollipop) and supports all common Android ABIs (armeabi-v7a, arm64-v8a, x86, x86_64).

Libraries
---------

The toolchain currently compiles the following libraries for Android:

* [GNUstep Base Library](https://github.com/gnustep/libs-base) (with some [patches](patches))
* [GNUstep CoreBase Library](https://github.com/gnustep/libs-corebase)
* [libobjc2](https://github.com/gnustep/libobjc2) (using gnustep-1.9 runtime)
* [libdispatch](https://github.com/apple/swift-corelibs-libdispatch) (official Apple release from the Swift Core Libraries)
* [libcxxrt](https://github.com/pathscale/libcxxrt) (for Objective-C++ exception support)
* [libffi](https://github.com/libffi/libffi)
* [libxml2](https://github.com/GNOME/libxml2)
* [libxslt](https://github.com/GNOME/libxslt)
* [ICU](https://github.com/unicode-org/icu)

Requirements
------------

Supported host platforms are macOS and Linux.

The following options need to be installed via the Android SDK Manager (e.g. via Android Studio):

* Android 5.0 (Lollipop / API level 21) SDK Platform _– or other SDK Platform as specified using `--level` option_
* Android SDK Build-Tools
* LLDB
* CMake _– version 3.10.2.4988404 as specified in [sdkenv.sh](env/sdkenv.sh)_
* Android SDK Platform-Tools
* Android SDK Tools
* NDK _– revision r19c (r20 not supported due to [NDK issue #1025](https://github.com/android-ndk/ndk/issues/1025))_

Usage
-----

Run the [build.sh](build.sh) script to build the toolchain:

```
Usage: ./build.sh
  -a, --abis ABI_NAMES    ABIs being targeted (default: "armeabi-v7a arm64-v8a x86 x86_64")
  -l, --level API_LEVEL   Android API level being targeted (default: 21)
  -b, --build BUILD_TYPE  Build type "Debug" or "Release" (default: Debug)
  -u, --no-update         Don't update projects to latest version from GitHub
  -c, --no-clean          Don't clean projects during build (e.g. for building local changes, only applies to first ABI being built)
  -p, --patches DIR       Apply additional patches from given directory
  -o, --only PHASE        Build only the given phase (e.g. "gnustep-base", requires previous build)
  -h, --help              Print usage information and exit
```

The toolchain is installed into the following location (`$GNUSTEP_HOME`):

* macOS: `~/Library/Android/GNUstep`
* Linux: `~/Android/GNUstep`

The build for each supported ABI is installed into its separate subfolder at that location (both libraries and header files differ per ABI).

To use the toolchain from an Android project, you can use `$GNUSTEP_HOME/$ABI_NAME/bin/gnustep-config` to obtain various flags that should be used to compile and link Objective-C files, e.g.

* `gnustep-config --variable=CC`
* `gnustep-config --objc-flags` (or `--debug-flags`)
* `gnustep-config --base-libs`

Call `gnustep-config --help` to obtain the full list of available variables.

Examples
--------

The [android-examples](https://github.com/gnustep/android-examples) repository contains example projects using this project.

Acknowledgements
----------------

Based on original work by Ivan Vučica.
