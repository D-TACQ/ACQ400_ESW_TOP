# ACQ400_ESW_TOP
Top level project to build ACQ400 Embedded SoftWare

### all components are git-submodules.


# To Clone ESW sources:

```
mkdir -p PROJECTS;cd PROJECTS

git clone --recurse-submodules https://github.com/D-TACQ/ACQ400_ESW_TOP.git
```

# Install Compiler:

Compiler: created on original build host like this:

```
tar cvzf xilinx-2018.3.tgz -C / --exclude=microblaze tools/Xilinx/SDK/2018.3/gnu
```

Install:
```
wget https://github.com/D-TACQ/ACQ400_ESW_TOP/releases/download/0.0.1/xilinx-2018.3.tgz
sudo tar xvzf  xilinx-2018.3.tgz -C/
```

## Host Packages
Redhat
```
sudo yum install uboot-tools
```
Ubuntu
```
sudo apt-get install u-boot-tools
```

# Build Components

## 0 cd ESW_TOP
```
cd ACQ400_ESW_TOP
```
## 1 Linux kernel

```
source ./setenv
cd acq400_kernel
cp d-tacq.config .config
./make.zynq oldconfig
./make.zynq uImage modules dtbs
cd ..
```

## 2 Initrd and Rootfs: buildroot

### Host Packages

Redhat
```
sudo yum install perl-ExtUtils-MakeMaker ncurses-devel
```
Ubuntu
```
sudo apt-get install libextutils-makemaker-cpanfile-perl
sudo apt-get install libncurses5-dev
```

### build
```
cd acq400_buildroot
make acq400_main_defconfig
make
cd ..
```

fails at pyepics?. just make again ...

```
result:
Image Name:   D-TACQ ACQ400 INITRD
Created:      Fri Apr  7 14:17:12 2023
Image Type:   ARM Linux RAMDisk Image (gzip compressed)
Data Size:    2982628 Bytes = 2912.72 KiB = 2.84 MiB
Load Address: 00000000
Entry Point:  00000000
POSTIMAGE99

peter@danna:~/PROJECTS/ACQ400_ESW_TOP/acq400_buildroot$ ls -l output/images/uramdisk.image.gz output/images/rootfs.ext2.gz 
-rw-r--r-- 1 peter peter 79829008 Apr  7 14:16 output/images/rootfs.ext2.gz
-rw-r--r-- 1 peter peter  2982692 Apr  7 14:17 output/images/uramdisk.image.gz
```

## 3 EPICS base 

```
(cd EPICS7; ./make.package)
```


## 4 ACQ400DRV
```
(cd ACQ400DRV; ./make.package)
```

## 5 CARRIER Specific packages
```
for P in ACQ400COMMON ACQ1001 ACQ1002 ACQ2x06 Z7IO; do (cd $P; ./make.package); done
```

## 6 MISC PACKAGES
```
for P in HTTPD ACQ400_TRANSIENT ACQ400_AI_MONITOR  ; do (cd $P; ./make.package); done
```


## 7 acq400ioc

### Host packages
Ubuntu
```
sudo apt-install libfftw3-dev libpcre++-dev
```
### Build
```
cd acq400ioc
./copy_libs
source ../EPICS7/base/setup.env
make
./make.package
cd ..
```

# Make a release

Only "master_version_host" can increment the VERSION number, but other hosts can create release tarballs for local use, and to prove that the distributed source is good. To make a "non master version host" release tarball.
```
(cd ACQ400RELEASE; ./scripts/make.release)
```
  - the script will prompt you to install an FPGA img file if a suitable img file is not already available.

The release appears like this:
```
RELEASE COMPLETE:
-rw-r--r-- 1 peter peter 122961920 Apr 10 17:15 REL/acq2x06_fpga-584-20230405215151.img
-rw-rw-r-- 1 peter peter 104601600 Apr 10 17:15 REL/acq400-584-20230410171508-danna.tar
```
 - the RELEASE tarball: eg acq400-VER-DATE-host.tar should be unpacked to the top level of the embedded SD card
 - the FPGA image eg acq2x06_fpga-VER-DATE.img should be copyed to the SD card ./ko directory

## To create a release SD card for Z7IO:
- follow extra steps in [Z7OIO INSTALL_SD.md](https://github.com/D-TACQ/Z7IO/blob/main/INSTALL_SD.md)

Please send any comments, queries, tips (hints)  to peter.milne@d-tacq.com







