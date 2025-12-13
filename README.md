# Build status

[![Build Status](https://dev.azure.com/dnceng-public/public/_apis/build/status/mono/mono-mono.posix?branchName=main)](https://dev.azure.com/dnceng-public/public/_build/latest?definitionId=189&branchName=main)

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [Build status](#build-status)
- [Mono.Posix standalone repository](#monoposix-standalone-repository)
    - [Supported operating systems](#supported-operating-systems)
    - [Supported managed target frameworks](#supported-managed-target-frameworks)
    - [Build requirements](#build-requirements)
    - [Source imports](#source-imports)
    - [Source changes relative to Mono sources](#source-changes-relative-to-mono-sources)
        - [Native code](#native-code)
        - [Managed code](#managed-code)

<!-- markdown-toc end -->

# Mono.Posix standalone repository

This repository contains `Mono.Posix` sources imported from the
[Mono](https://github.com/mono/mono) repository in order to continue
active maintenance and development of the assembly and its support
library outside Mono proper.  The goal is to make the assembly
available as a universal nuget that works across a number of Unix and
Unix-like systems.

This repository should be treated as the "canonical" source of
`Mono.Posix` - all new code, bug fixes etc should be implemented here.
Mono copy of the code will not receive any new updates or fixes.

## Supported operating systems

This fork aims only to generate the Mono.Unix library and its respective native component so that it can be used in systems with RISCv64 architecture.

  * **Linux** (binaries are built on Debian 13 currently)
	* Supported architectures: `riscv64`
	
Builds for Linux and macOS produce static and dynamic libraries, all
the other builds produce only static libraries.  Dynamic libraries do
not have any additional dependencies except for the C library.

## Supported managed target frameworks

  * `Mono.Posix` and `Mono.Unix` support
    * `net10.0`
  * `Mono.Unix.Tests` supports
    * `net10.0`

## Build requirements

  * All platforms
    * `dotnet` (10.*)
	* `cmake` (3.18 at the minimum)
    * `ninja` (any version supported by `cmake`)
	* C++ compiler with support for the C++17 standard
  * Linux
    * `gcc` or `clang` compilers

## Source imports

Repository contains `Mono.Posix` and its native sources helper sources imported from
the https://github.com/mono/mono/ repository, commit 0bc0f6bef0c05d3367c82d2e414e9db256ae1541

The `src/Mono.Posix` directory here corresponds to `mcs/class/Mono.Posix/` in the Mono repository.

The `src/native` directory here corresponds to `support/` in the Mono repository

## Source changes relative to Mono sources

### Native code

All the native code was migrated to C++ (using C++17 standard) and all
functions not used by the managed code were removed.  As the result of
this migration, the native code uses stronger typing and stricter
compile-time error checking.

### Managed code

`Mono.Posix.dll` is now obsolete, with all the code that used to be in
it moved to the new `Mono.Unix.dll` assembly.  `Mono.Posix.dll`
contains only type forwarders for the moved types and a handful of
classes which were previously marked obsolete - they will now produce
build errors instead of warnings, if used by the application
referencing the new `Mono.Posix`.
