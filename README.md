# cpp-demo
Checking conan and cmake with github apps

Windows: [![Build status](https://ci.appveyor.com/api/projects/status/f6pnc2gkfs97blvr/branch/master?svg=true)](https://ci.appveyor.com/project/Krzysztow/cpp-demo/branch/master)

Linux (TODO OSX): [![Build Status](https://travis-ci.org/Krzysztow/cpp-demo.svg?branch=master)](https://travis-ci.org/Krzysztow/cpp-demo)

# Preparation of the build environment in general

In order to be able to build this project, one has to setup his build environment. That contains of following components:
* working C/C++ compiler (gcc/clang/msvc)
* cmake build generator
* conan package manager

We've chosen to use cmake, as it is a de-facto standard in the C++. If developing on one machine/one environment, one doesn't need a build generator system. They could use the particular build system of their choice. However, once you want to build on multiple operating systems or even compilers, the build generator like cmake comes in handy. It detects different build scenarios and applies sane values (e.g. libraries it needs extra to link to) and generates the build configuration for a targeted build system (e.g. Makefiles or sln projects). So, by having cmake, you maintain just one source defining your build.

The conan package manager is chosen as a preference although there exist different options (e.g. VCPKG, Hunter (for CMake), Buckaroo, each has it's pros and cons, you need to check it yourself). Conan might already have binaries of the libraries precompiled for your platform. In such a case, it just downloads them. Otherwise, you migth instruct it to get them built. Additionally, we've chosen to use conan integration into CMake [cmake-conan](https://github.com/conan-io/cmake-conan/). This way, we don't have to invoke conan commands externally, and cmake takes care of that on its own (also making sure the conan environment matches the build environment).

For a discussion on comparison of the package managers, please see [this reddit here](https://www.reddit.com/r/cpp/comments/8t0ufu/what_is_a_good_package_manager_for_c/)

## Preparation of the Ubuntu machine

## Preparation of the Arch machine

## Preparation of the MacOS machine

## Preparation of the Windows machine

