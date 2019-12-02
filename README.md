giflib library build for Staticlibs
===================================

[![travis](https://travis-ci.org/staticlibs/external_giflib.svg?branch=master)](https://travis-ci.org/staticlibs/external_giflib)
[![appveyor](https://ci.appveyor.com/api/projects/status/github/staticlibs/external_giflib?svg=true)](https://ci.appveyor.com/project/staticlibs/external-giflib)

This project is a part of [Staticlibs](http://staticlibs.net/).

This project contains a CMake build file for building the [giflib](http://giflib.sourceforge.net/) library that
can be used to build sources imported from [CentOS lookaside](https://github.com/staticlibs/lookaside_giflib.git).

How to build
------------

[CMake](http://cmake.org/) is required for building.

To build the library on Windows using Visual Studio 2013 Express run the following commands using
Visual Studio development command prompt 
(`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\Tools\Shortcuts\VS2013 x86 Native Tools Command Prompt`):

    git clone https://github.com/staticlibs/external_giflib.git
    git clone https://github.com/staticlibs/lookaside_giflib.git
    cd external_giflib
    mkdir build
    cd build
    cmake ..
    msbuild external_giflib.sln

See [StaticlibsToolchains](https://github.com/staticlibs/wiki/wiki/StaticlibsToolchains) for 
more information about the CMake toolchains setup and cross-compilation.

License information
-------------------

This project is released under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0).

Changelog
---------

**2019-12-02**

 * version 4.1.6-3
 * support overriding C runtime lib on Windows

**2018-03-05**

 * version 4.1.6-2
 * drop submodule

**2016-12-18**

 * version 4.1.6-1
 * lookaside build

**2016-02-04**

 * version 5.1.2.0
 * build for Linux and Windows
