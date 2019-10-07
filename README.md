# cpp-demo
Checking conan and cmake with github appsWARNING

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

**Notes:**
Conan is a commcand line tool written in python. Hence, we'll have to make sure we have python (preferably version 3; version 2 is ending its life anyway).c

## Preparation of the Ubuntu machine
You have to have administrator priviledges to execute these commands.

```
# install build tools and gcc compiler
sudo apt install -y build-essential

# install clang compiler (it's good to have choice... 
# and we want to have option of the same compiler on 
# different platforms)
sudo apt install -y clang-9
# make clang/clang++ v9 available on your path as clang/clang++
update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-9 100
update-alternatives --install /usr/bin/clang clang /usr/bin/clang-9 100

# install CMake; we use curl to download the linux archive; 
# cmake has apt repositories but currently none for the newest Ubuntu disco
apt install -y curl
CMAKE_VERSION=3.14.7-Linux-x86_64
curl -sSL https://github.com/Kitware/CMake/releases/download/v3.14.7/cmake-${CMAKE_VERSION}.tar.gz | \
  sudo tar -xzC /opt # if installed elsewhere, no sudo might be needed 
echo "Update your ~/.profile with:"
echo "export PATH="/opt/cmake-${CMAKE_VERSION}/bin/:\$PATH"

# install conan
pip3 install --user conan
echo "Update your ~/.profile with:"
echo "export PATH="~/.local/bin:\$PATH"
```

## building with gcc
```
mkdir gcc-build && cd gcc-build
# configure and generate build scipts (Makefiles by default)
CC=/usr/bin/gcc \
  CXX=/usr/bin/g++ \
  cmake --configure <path=to-the-project> -DCMAKE_BUILD_TYPE=Debug
# perform the build
cmake --build .
# run tests
ctest
```

## building with clang
```
mkdir clang-build && cd clang-build
# configure and generate build scipts (Makefiles by default)
CC=/usr/bin/clang \
  CXX=/usr/bin/clang++ \
  cmake --configure <path=to-the-project> -DCMAKE_BUILD_TYPE=Debug
# perform the build
cmake --build .
# run tests
ctest
```

## Preparation of the Arch machine

## Preparation of the MacOS machine

## Preparation of the Windows machine

## Explanation of the provided demo project structureapt install -y build-essential
