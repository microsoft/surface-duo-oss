# Building Surface Duo Kernel

## Install Ubuntu 18.04
(These steps were built off of the default Azure Virtual Machine running Ubuntu 18.04 LTS Server; however many of us use Ubuntu Desktop
& Server 18.04, 19.10, and 20.04)

You will need about 100 GB’s of disk-space for this.

### Ensure Ubuntu is up to date

```
apt-get update
apt-get upgrade
```

### Install pre-requisites

```
apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc unzip libssl-dev libxml-simple-perl \
libc++1 libc++-dev python bc filter
```

## Setup git-repo

We have setup a kernel manifest using git-repo tools to download source
from Azure DevOps and additional tools from http://android.googlesource.com/

```
mkdir ~/bin
PATH=~/bin:$PATH
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

## Get the kernel source
Create folder to download the repos to

```
mkdir ~/repo
```

Setup source

```
cd ~/repo
repo init -u https://github.com/microsoft/surface-duo-oss-kernel.manifest -b *BRANCH*
repo sync
```

### Surace Duo Kernel Releases

| Build Number | repo branches |
|-|-|
| 2020.618.244 | surfaceduo/10/2020.618.244 |
| 2020.618.245 | surfaceduo/10/2020.618.245 |
| 2020.618.275 | surfaceduo/10/2020.618.275 |
| 2020.723.130 | surfaceduo/10/2020.723.130 |
| 2020.812.86 | surfaceduo/10/2020.812.86 |
| 2020.812.87 | surfaceduo/10/2020.812.87 |
| 2020.910.72 | surfaceduo/10/2020.910.72 |
| 2020.910.73 | surfaceduo/10/2020.910.73 |
| 2020.1014.61 | surfaceduo/10/2020.1014.61 |
| 2020.1014.64 | surfaceduo/10/2020.1014.64 |
| 2020.1110.124 | surfaceduo/10/2020.1110.124 |
| 2020.1110.126 | surfaceduo/10/2020.1110.126 |
| 2020.1211.85 | surfaceduo/10/2020.1211.85 |
| 2020.1211.86 | surfaceduo/10/2020.1211.86 |
| 2020.1211.87 | surfaceduo/10/2020.1211.87 |
| 2021.115.52 | surfaceduo/10/2021.115.52 |
| 2021.115.53 | surfaceduo/10/2021.115.53 |
| 2021.115.54 | surfaceduo/10/2021.115.54 |

## Get the Qualcomm Snapdragon LLVM Compiler

Download the Snapdragon LLVM Compiler for Android 8.0.6 - Linux64 (may require registration) from Qualcomm Developer Network - https://developer.qualcomm.com/software/snapdragon-llvm-compiler-android/tools 

The default build scripts expect a specifc folder under the repo root. 
Create the base folder:(or modify kernel/msm-4.14/build.config.common)
```
mkdir -p ~/repo/proprietary/llvm-arm-toolchain-ship/8.0
```

Uncompress the file snapdragon-llvm-8.0.6-linux64.tar.gz
```
cp -r ~/<uncompressed dir>/toolchains/llvm-Snapdragon_LLVM_for_Android_8.0/prebuilt/linux-x86_64/* ~/repo/proprietary/llvm-arm-toolchain-ship/8.0/
```

## Build

```
export BUILD_CONFIG=./kernel/msm-4.14/build.config.perf-config
build/build.sh
```

The output will be written to ./out/ and the out/dist can be used on the device or with an AOSP build environment.


## Additional resources

Android Building Kernels - https://source.android.com/setup/build/building-kernels
