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
sudo tar xvzf tools/Xilinx/SDK/2018.3/gnu -C/
```



