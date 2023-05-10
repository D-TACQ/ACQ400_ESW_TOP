# Workflow to update a package

[pgm@hoy5 ACQ420FMC]$ git push origin main2
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
To github.com:petermilne/ACQ420FMC.git

Sync github.com/D-TACQ/ACQ400DRV.git

git submodule update --remote ACQ400DRV

git commit -a -m "update"


