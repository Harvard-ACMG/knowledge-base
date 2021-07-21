# Harvard-ACMG resources on Cannon

This page summarizes resources available to Harvard-ACMG group members.  We also recommend that you read the [Cluster Storage](https://docs.rc.fas.harvard.edu/kb/cluster-storage/) page on the FASRC documentation site.

## Dedicated partitions

Harvard-ACMG group members can [submit computational jobs](using-slurm-on-cannon-2.md) to the [HUCE Partitions](https://docs.rc.fas.harvard.edu/kb/huce-partitions/s/slurm#the_huce_climate_partitions).  These partitions have no memory or time limits, and can support long model simulations.

## Storage space

### Home directory

The `/n/home*` directories are your primary storage location for analysis and data processing. Your home directory is the initial working directory upon login and can always be returned to by typing `cd ~`. Every Cannon user has a 100 GB size limit in their home directory. This directory is backed up daily. You can find the address of another user's
home directory by typing `file ~username`, filling in the username of the person whose directory you are looking for.

### Shared scratch storage

The home directory may be sufficient for most tasks. However, if your job will produce many files and/or large files, then we recommend using the scratch filesystems offered on Cannon. The scratch filesystem is ideal for I/O intensive jobs. The Jacob Group has shared scratch storage located in:

```
$SCRATCH/jacob_lab/
```

where `$SCRATCH` is a system-defined environment variable that points to the root scratch disk directory. (As of Mar 2020, `$SCRATCH` points to `/n/holyscratch01`, but this could change in the future.) We recommend that you create the subdirectory `$SCRATCH/jacob_lab/YOUR-USERNAME` in which to run your simulations and/or do your data analysis.

There is a 90 day time limit for files on the `$SCRATCH` disk space and a 100(?) TB limit per group. However, if the `$SCRATCH` disk starts to approach its storage capacity, Research Computing will begin deleting files that are older than 90 days. This space is not backed up.

NOTE: If the `$SCRATCH` disk fills up to ~95% of full, FASRC will start removing older files in order to prevent degradation of performance.  Therefore, consider moving all files off `$SCRATCH` into storage as soon as you can.

### Long-term storage

The Jacob Group has long-term storage space in:
```
/n/seasasfs02/
```
Each Jacob Group user has a folder in that location with a quota of 500 GB. There is also a `share` folder which can be used to share files with other group members. This space is backed up.

## Shared data directories

The GEOS-Chem data directories are located at:
```
/n/holylfs/EXTERNAL_REPOS/GEOS-CHEM/gcgrid/data/ExtData
```
The first time you create a GEOS-Chem run directory, you will be asked to supply this path.  It will be stored in your `~/.geoschem/config` file for subsequent use.