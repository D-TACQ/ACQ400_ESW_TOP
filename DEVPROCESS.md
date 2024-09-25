# DEVPROCESS as seen on Primary D-TACQ host.
## If that's not you, see README.md

# Primary package repos:
```
~/PROJECTS/ACQ400/ACQ400DRV
url = git@github.com:D-TACQ/ACQ400DRV.git
```
ie Write access to github.

Happy with package?. Push:
```
pgm@hoy6:~/PROJECTS/ACQ400/ACQ400DRV$ git push origin main2
Enumerating objects: 20, done.
Counting objects: 100% (20/20), done.
...
To github.com:D-TACQ/ACQ400DRV.git
   5d0045c..25ba3e4  main2 -> main2
```

# Primary Top Level
```
~/PROJECTS/ACQ400/ACQ400_ESW_TOP
url = git@github.com:D-TACQ/ACQ400_ESW_TOP.git
```

ie Write access to github.

pgm@hoy6:~/PROJECTS/ACQ400/ACQ400_ESW_TOP$ git submodule update --remote ACQ400DRV
remote: Enumerating objects: 28, done.
remote: Counting objects: 100% (28/28), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 21 (delta 13), reused 18 (delta 10), pack-reused 0 (from 0)
Unpacking objects: 100% (21/21), 2.84 KiB | 161.00 KiB/s, done.
From https://github.com/D-TACQ/ACQ400DRV
   ae6540d..25ba3e4  main2      -> origin/main2
Submodule path 'ACQ400DRV': checked out '25ba3e443039b194abc8c86462939e0f9922c3f3'

## ie same update as above.

## Commit and push:
```
pgm@hoy6:~/PROJECTS/ACQ400/ACQ400_ESW_TOP$ git commit -a
[main 18a4cd1] ACQ400DRV updated clean dsp init for generic DSP in acq1102 	modified:   ACQ400DRV
 1 file changed, 1 insertion(+), 1 deletion(-)
pgm@hoy6:~/PROJECTS/ACQ400/ACQ400_ESW_TOP$ git push origin main
..
To github.com:D-TACQ/ACQ400_ESW_TOP.git
   f29b3ed..18a4cd1  main -> main
```
# Go to release ESW_TOP

## Pull update
```
cd ~/PROJECTS/ACQ400_ESW_TOP
pgm@hoy6:~/PROJECTS/ACQ400_ESW_TOP$ git pull origin main
..   
Fetching submodule ACQ400DRV
From https://github.com/D-TACQ/ACQ400DRV
   ae6540d..25ba3e4  main2      -> origin/main2
Updating f29b3ed..18a4cd1
Fast-forward
 ACQ400DRV | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```
## Update the module
```
pgm@hoy6:~/PROJECTS/ACQ400_ESW_TOP$ git submodule update ACQ400DRV
Submodule path 'ACQ400DRV': checked out '25ba3e443039b194abc8c86462939e0f9922c3f3'
```
## build the module
```
pushd ACQ400DRV/
./make.zynq package
```
# Now build Master Release
```
cd ACQ400RELEASE.M/
./scripts/make.release

scp REL/acq400-712-20240925153818.tar REL/fpga-712-20240925153818.img dt100@eigg:/home/dt100/PROJECTS/ACQ400/RELEASE/REL
ssh dt100@eigg /home/dt100/bin/set.acq4xx.current acq400-712-20240925153818.tar
```



