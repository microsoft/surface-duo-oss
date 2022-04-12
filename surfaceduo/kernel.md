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
| 2021.207.70 | surfaceduo/10/2021.207.70 |
| 2021.207.71 | surfaceduo/10/2021.207.71 |
| 2021.207.72 | surfaceduo/10/2021.207.72 |
| 2021.314.91 | surfaceduo/10/2021.314.91 |
| 2021.314.92 | surfaceduo/10/2021.314.92 |
| 2021.314.93 | surfaceduo/10/2021.314.93 |
| 2021.419.70 | surfaceduo/10/2021.419.70 |
| 2021.419.71 | surfaceduo/10/2021.419.71 |
| 2021.419.72 | surfaceduo/10/2021.419.72 |
| 2021.525.62 | surfaceduo/10/2021.525.62 |
| 2021.525.63 | surfaceduo/10/2021.525.63 |
| 2021.525.64 | surfaceduo/10/2021.525.64 |
| 2021.622.32 | surfaceduo/10/2021.622.32 |
| 2021.622.45 | surfaceduo/10/2021.622.45 |
| 2021.622.47 | surfaceduo/10/2021.622.47 |
| 2021.721.41 | surfaceduo/10/2021.721.41 |
| 2021.721.42 | surfaceduo/10/2021.721.42 |
| 2021.721.43 | surfaceduo/10/2021.721.43 |
| 2021.817.35 | surfaceduo/10/2021.817.35 |
| 2021.817.36 | surfaceduo/10/2021.817.36 |
| 2021.817.37 | surfaceduo/10/2021.817.37 |
| 2021.913.25 | surfaceduo/10/2021.913.25 |
| 2021.913.26 | surfaceduo/10/2021.913.26 |
| 2021.913.27 | surfaceduo/10/2021.913.27 |
| 2021.1019.24 | surfaceduo/10/2021.1019.24 |
| 2021.1019.25 | surfaceduo/10/2021.1019.25 |
| 2021.1019.26 | surfaceduo/10/2021.1019.26 |
| 2021.1027.156 | surfaceduo/11/2021.1027.156 |
| 2022.111.64 | surfaceduo/11/2022.111.64 |
| 2022.113.26 | surfaceduo/11/2022.113.26 |
| 2022.115.10 | surfaceduo/11/2022.115.10 |

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

