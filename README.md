# Building a full embedded ARM toolchain on Windows using Cygwin

## Overview

## Why Cygwin

## Installing

To build the toolchain, you will need the following package from the Cygwin
installer:

-   `gcc-core`
-   `gcc-g++`
-   `make`
-   `bison`
-   `flex`
-   `perl`
-   `libgmp-devel`
-   `libmpfr-devel`
-   `zlib-devel`
-   `libmpc-devel`
-   `libisl-devel`
-   `libexpat-devel`

You can then clone the repository as well as the submodules containing the
required sources. This might take a couple of minutes as the dependencies are
quiet large.

```shell
git clone https://github.com/mkende/cygwin-arm-toolchain.git
cd cygwin-arm-toolchain
git submodule update --init
```

## Building the toolchain

Just run the `./build-toolchain` command. This will build and install a complete
ARM toolchain in your Cygwin environment. By default the toolchain is installed
under `/usr/local/bin` which should already be in your `$PATH` variable.

You can execute `./build-toolchain -h` to see a list of options affecting the
program.

## Updating

## Tested