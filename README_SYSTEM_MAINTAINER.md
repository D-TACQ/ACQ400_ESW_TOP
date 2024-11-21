# SYSTEM MAINTAINER GUIDE

**WD**: Working Directory : created by git clone
**RO**: Read Only Permission
**WR**: Write Permission

## Role1 : BUILDER

**BUILDERS** don't need any privilege at all, build as described in README.
All repos are https://, ie public.

But, how did all this come to be?. Someone has to develop it:

## Role0 : SYSTEM MAINTAINER SM

In addition to the set of **RO** working directories,
SM Keeps a set of local working directories with **WR** WRITE access to the server.
That INCLUDES **ACQ400_ESW_TOP**

Preferred directory structure:

```
HP=$HOME/PROJECTS
```
copy of public build area:
```
$HP/ACQ400_ESW_TOP     # .git/config -> https:// RO
```

maintainer projects:
```
$HP/ACQ400/            # ACQ400 DEVELOPMENT area with WD of MODULES, eg
                              

$HP/ACQ400/ACQ400DRV   # .git/config -> git://  RW
$HP/ACQ400/acq400ioc   # .git/config -> git://  RW

### Development Repo for top level project
$HP/ACQ400/ACQ400_ESW_TOP # .git/config -> git://  RW
```
nb: these two directories may have the same content, but very different purpose:
```
$HP/ACQ400_ESW_TOP     # .git/config -> https:// RO
$HP/ACQ400/ACQ400_ESW_TOP # .git/config -> git://  RW
```
The latter is controls the versions of all submodules; the former is the build area for creating releases.

### Sample workflow

1. Make change in $HP/ACQ400/ACQ400DRV, test, push
```
vi *.c; ./make.zynq; ./make.zynq package; update package, test.
git push origin main
```

2. Update submodule version in $HP/ACQ400/ACQ400_ESW_TOP:
```
git submodule update --remote ACQ400DRV
git commit -a -m "message"
git push origin main
```

3. Now rebuild in public release area $HP/ACQ400_ESW_TOP
```
git pull origin main
git submodule update ACQ400DRV
(cd ACQ400DRV; ./make.package)
(cd ACQ400RELEASE; ./scripts/make.release)
```
## SM RELEASE area.
```
The SYSTEM MAINTAINER needs a WD with WR access to the ACQ400RELEASE repo, so that it can increment the .version file.
This WD lives in the SM's public release area, so that it can collate ALL submodules, but it's not itself a submodule!

cd $HP/ACQ400_ESW_TOP
git clone git://github/COMPANY/ACQ400RELEASE ACQ400RELEASE.M     # rename to distinguish from the public RO version
(cd ACQ400RELEASE.M;./scripts/make.release)                      # is able to increment the global .version file
```
### Note to self: after makeing the release, rememebr to push ACQ400RELEASE to that regular public builders get the uptodate version
```
(cd ACQ400RELEASE.M; git push origin master)

cd $HP/ACQ400_ESW_TOP;
git submodule update --remote ACQ400RELEASE
git commit -a -m "new release"
git push origin main
```
### now BUILDERS can pick up the latest version for their local builds.




