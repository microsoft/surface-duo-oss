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

### Surface Duo 2 Kernel Releases

| Build Number | repo branches |
|-|-|
| 2021.806.220 | surfaceduo2/11/2021.806.220 |
| 2021.824.206 | surfaceduo2/11/2021.824.206 |
| 2021.827.34 | surfaceduo2/11/2021.827.34 |
| 2021.830.19 | surfaceduo2/11/2021.830.19 |
| 2021.923.272 | surfaceduo2/11/2021.923.272 |
| 2021.1116.111 | surfaceduo2/11/2021.1116.111 |
| 2022.104.111 | surfaceduo2/11/2022.104.111 |
| 2022.106.38 | surfaceduo2/11/2022.106.38 |
| 2022.108.8 | surfaceduo2/11/2022.108.8 |
| 2022.110.15 | surfaceduo2/11/2022.110.15 |
| 2022.418.98 | surfaceduo2/11/2022.418.98 |
| 2022.517.98 | surfaceduo2/11/2022.517.98 |
| 2022.519.47 | surfaceduo2/11/2022.519.47 |
| 2022.521.8 | surfaceduo2/11/2022.521.8 |
| 2022.815.179 | surfaceduo2/11/2022.815.179 |
| 2022.817.23 | surfaceduo2/11/2022.817.23 |
| 2022.819.57 | surfaceduo2/11/2022.819.57 |

## Build

Build Command:
 ```
./device_build/kernel/kernel-build.sh -c [FLAVOR] -tb [user|userdebug] -p [BUILD_ROOT_PATH]
 ```

Flavors:
- lahaina-gki_defconfig (user)
- lahaina-qgki_defconfig (user | userdebug)
 
Example Build GKI:
```
./device_build/kernel/kernel-build.sh -c lahaina-gki_defconfig -tb user -p ~/repo
```
 
Example Build QGKI User and UserDebug:
```
./device_build/kernel/kernel-build.sh -c lahaina-qgki_defconfig -tb user -p ~/repo
```
```
./device_build/kernel/kernel-build.sh -c lahaina-qgki_defconfig -tb userdebug -p ~/repo
```

The output will be written to ./out/ and the out/dist can be used on the device or with an AOSP build environment.
