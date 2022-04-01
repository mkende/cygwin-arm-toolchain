# Building a full embedded ARM toolchain on Windows using Cygwin

## Overview

This tool can be used to build a full ARM toolchain under Cygwin. The tool
initially had patch files specific to build on Cygwin, but these bugs were fixed
in recent version of the GNU tools so this program is actually not specific to
Cygwin in its current state. So it can maybe be used on other platforms. But
these other platforms usually have pre-packaged version of the toolchain that
will be more conveniant to use (and this tool is tested only under Cygwin).

The toolchain built by this  tool have been used to build programs for the
Raspberry Pi RP2040 using the Pico SDK and for various Atmel SAMD devices using
the Atmel Start framework.

## Why Cygwin

For peoples that have or prefer to use Windows, there are these days many
options (full virtualization, Windows Subsystem for Linux, native toolchains,
etc.). However, I find that Cygwin has the best balance between a _Linux_
look-and-feel and a good  and fast integration with the standard Windows system.

Within Cygwin, it would be possible to use a Windows toolchain. However a Cygwin
native one is much simpler to use (no complex file path conversion) and usually
compatible with more open source projects and tools.

## Installing

To build the toolchain, you will need the following package from the Cygwin
installer:

-   `gcc-core`
-   `gcc-g++`
-   `make`
-   `bison`
-   `flex`
-   `perl`
-   `patch`
-   `git`
-   `libgmp-devel`
-   `libmpfr-devel`
-   `zlib-devel`
-   `libmpc-devel`
-   `libisl-devel`
-   `libexpat-devel`

You can then clone the repository as well as the submodules containing the
required sources. This might take a couple of minutes as the dependencies are
quite large:

```shell
git clone https://github.com/mkende/cygwin-arm-toolchain.git
cd cygwin-arm-toolchain
git submodule update --init
```

## Building the toolchain

Just run the `./build-toolchain` command. This will build and install a complete
ARM toolchain in your Cygwin environment. By default the toolchain is installed
under `/usr/local/bin` which should already be in your `$PATH` variable.

The build operation is organized in _projects_ that you can individually
activate or deactivate, although you should generally just build everything with
the default options. The projects, all targetted to the ARM architecture, are
the following:

-   `binutils`: a standard set of GNU tools (the most important ones are the 
    linker `ld` and the debugger `gdb`). These tools are running on your
    computer so they are built with your default GCC.
-   `gcc-bootstrap`: a bootstrap version of the GCC compiler with only the
    support for C. This is skipped if there is already an ARM GCC available on
    the system (typically from a previous run).
-   `newlib`: a small standard library, meant for embedded systems.
-   `gcc`: the full version, with support for C and C++. It requires `newlib` to
    be built.
-   `newlib-nano`: an even smaller version of `newlib`.
-   `newlib-final`: just the same as `newlib` but built again with our full
    `gcc`. This is skipped if `gcc-bootstrap` was built too (as `newlib` was
    built with the same version of GCC in that case).

You can execute `./build-toolchain -h` to see a list of options affecting the
program.

## Cleanup

Just delete the `build` directory within the project to remove all build
artifacts. If you use the `--build-here` argument then, obviously, you should
remove any directories you used to build.

## Bugs

The various projects that are built are not always tested with Cygwin so they
may fail to build. By using the versions specified in this repository you should
be using known-good versions (you are getting them by default when this
repository is cloned). If needed patches will be provided with this tool to
allow a successful compilation under Cygwin.

## Updating

TODO