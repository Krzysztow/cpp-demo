# cpp-demo
Checking conan and cmake with github appsWARNING

Windows: [![Build status](https://ci.appveyor.com/api/projects/status/f6pnc2gkfs97blvr/branch/master?svg=true)](https://ci.appveyor.com/project/Krzysztow/cpp-demo/branch/master)

Linux (TODO OSX): [![Build Status](https://travis-ci.org/Krzysztow/cpp-demo.svg?branch=master)](https://travis-ci.org/Krzysztow/cpp-demo)

# Preparation of the build environment in general

To download this project, best get `git` and do `git clone <project-url>` (or use your client tool [SmartGit](https://www.syntevo.com/smartgit/), [GitKraken](https://www.gitkraken.com/), [Fork](https://git-fork.com/), [Tig](https://github.com/jonas/tig) or others).

In order to be able to build this project, one has to setup his build environment. That contains of following components:
* working C/C++ compiler (gcc/clang/msvc)
* cmake build generator
* conan package manager

We've chosen to use cmake, as it is a de-facto standard in the C++. If developing on one machine/one environment, one doesn't need a build generator system. They could use the particular build system of their choice. However, once you want to build on multiple operating systems or even compilers, the build generator like cmake comes in handy. It detects different build scenarios and applies sane values (e.g. libraries it needs extra to link to) and generates the build configuration for a targeted build system (e.g. Makefiles or sln projects). So, by having cmake, you maintain just one source defining your build.

The conan package manager is chosen as a preference although there exist different options (e.g. VCPKG, Hunter (for CMake), Buckaroo, each has it's pros and cons, you need to check it yourself). Conan might already have binaries of the libraries precompiled for your platform. In such a case, it just downloads them. Otherwise, you migth instruct it to get them built. Additionally, we've chosen to use conan integration into CMake [cmake-conan](https://github.com/conan-io/cmake-conan/). This way, we don't have to invoke conan commands externally, and cmake takes care of that on its own (also making sure the conan environment matches the build environment).

For a discussion on comparison of the package managers, please see [this reddit here](https://www.reddit.com/r/cpp/comments/8t0ufu/what_is_a_good_package_manager_for_c/)d:/llvm/bin/

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

### building with gcc
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

### building with clang
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
 versions that may have System Integrity Protection, pip may fail. Try using virtualenvs, or install with another user $ 
## Preparation of the MacOS machine
* Get XCode command line tools - https://www.embarcadero.com/starthere/xe5/mobdevsetup/ios/en/installing_the_commandline_tools.html
* Get Brew (package manager) - https://docs.brew.sh/Installation
* Install cmake - `brew install cmake`
* Install conan - `python install --user conan` (potentially need to reload profile)
TODO: verify on clean Mac

## Preparation of the Windows machine
### Getting compilers and cmake:
The best option is to install the tools from the newest Visual Studio 2019. However, for this you have to make sure your Windows is updated. The installation instructions are [here](https://docs.microsoft.com/en-gb/cpp/build/vscpp-step-0-installation?view=vs-2019).
When appropriate components are choo110
​
111
To install install llvm 9.0.0 (at the moment of writing, that's the newest realease) sen (TODO: put which ones, minimal setup?) we should have msvc compiler, cmake and other tools needed.

#### Command lines
* We use [chocolatey](https://chocolatey.org/) (package manager) to get the tools - paste a command line from [here](https://chocolatey.org/docs/installation)
* Install MSVC compiler, Clang, CMake and other tools from the VisualStudio package:
```
choco install visualstudio2019buildtools --passive --force --package-parameters "--add Microsoft.VisualStudio.Workload.VCTools;includeRecommended;includeOptional"
```
* If you don't want to install CMake or LLVM from VSTools, choco has new version
```
choco install cmake
choco install llvm
```
#### Manual steps
* Download VS Build Tools from [VS Download Page](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019). Search for "Build Tools for Visual Studio 2019" in all downloads. Run the installer - choose "C++ build tools", and "C++ CMake tools for Windows" + "C++ Clang tools for Windows" if you want to get these components from VS 
* If we want to compile using newer version of clang, (at the momoent of writing VS ships clang 8), get the installer from [here](https://releases.llvm.org/9.0.0/LLVM-9.0.0-win64.exe). During installation, you'll be asked for an installation location and if you want to add LLVM path to the system environment variable PATH. If you don't do it, make sure you remember that path (default `C:\Program Files\LLVM`) to add it per each compilation step.

### Conan installation
You need to make sure if Python3 is installed (best with python on the path already). Either from [Python download page](https://www.python.org/downloads/) or 
```
choco install pythono
```

Then, then on the command line follow the same steps as on linux:
```
REM use below if python path is not set and it's installed in e.g. C:\Python37
REM set PATH=C:\Python37;%PATH%
pip3 install conan
conan --version
```
### building with msvc
```
mkdir build && cd build
REM Populate build environment for appropriate target
"C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\Tools\VsDevCmd.bat" -host_arch=amd64
REM configure and generate build scipts
cmake \
  -DCMAKE_BUILD_TYPE=Debug \
  -G "Visual Studio 16 2019" \
  --configure <path=to-the-project>
# perform the build
cmake --build .
# run tests
ctest
```

### building with clang
```
mkdir clang-build && cd clang-build
REM make sure clang is visible on the PATH
set PATH=C:\Program Files\LLVM\bin;%PATH%
REM Populate build environment for appropriate target
"C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\Tools\VsDevCmd.bat" -host_arch=amd64
REM configure and generate build scipts
cmake ^
  -DCMAKE_C_COMPILER=clang-cl.exe -DCMAKE_CXX_COMPILER=clang-cpp.exe ^
  -DCMAKE_BUILD_TYPE=Debug ^
  -G "Visual Studio 16 2019" ^
  --configure <path=to-the-project>
# perform the build
cmake --build .
# run tests
ctest
```
## Explanation of the provided demo project structure
