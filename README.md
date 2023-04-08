# ACQ400_ESW_TOP
Top level project to build ACQ400 Embedded SoftWare

# all components are submodules.


# To build ESW:

```
git clone --recurse-submodules https://github.com/D-TACQ/ACQ400_ESW_TOP.git
```

# To update

```
git submodule update --remote acq400_kernel
```



Tools: created:
tar cvzf xilinx-2018.3.tgz -C / --exclude=microblaze tools/Xilinx/SDK/2018.3/gnu

install:
```
wget https://github.com/D-TACQ/ACQ400_ESW_TOP/releases/download/0.0.1/xilinx-2018.3.tgz

sudo yum install uboot-tools
sudo apt-get install u-boot-tools

sudo tar xvzf  xilinx-2018.3.tgz -C/
source ./setenv
cd acq400_kernel
cp d-tacq.config .config
./make.zynq oldconfig
./make.zynq uImage modules dtbs
```

# buildroot

```
sudo yum install perl-ExtUtils-MakeMaker ncurses-devel

sudo apt-get install libextutils-makemaker-cpanfile-perl
sudo apt-get install libncurses5-dev
cd acq400_buildroot

make acq400_main_defconfig
make

## fails at pyepics?. just make again ...

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

# EPICS base 
## follow EPICS/acq400_epics_base/README.ACQ400

# ACQ400DRV
mkdir PACKAGES
source ./setenv
cd ACQ400DRV
make
./make.zynq
./make.zynq package

# CARRIER Specific packages
for P in ACQ1001 ACQ1002 ACQ2x06 Z7IO; do (cd $P; ./make.package); done

# MISC PACKAGES

for P in HTTPD ACQ400_TRANSIENT ACQ400_AI_MONITOR  ; do (cd $P; ./make.package); done


# acq400ioc

sudo apt-install libfftw3-dev libpcre++-dev

cd acq400ioc
./copy_libs
source ../EPICS7/base/setup.env
make
./make.package






