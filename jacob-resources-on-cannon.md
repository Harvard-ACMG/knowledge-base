# Jacob Group resources on Cannon (formerly Odyssey)

This page summarizes resources available to the Jacob Group on Cannon.

## Storage space

### Home directory

The `/n/home*` directories are your primary storage location for
analysis and data processing. Your home directory is the initial working
directory upon login and can always be returned to by typing `cd ~`.
Every Cannon user has a 100 GB size limit in their home directory. This
directory is backed up daily. You can find the address of another user's
home directory by typing `file ~username`, filling in the username of
the person whose directory you are looking for.

### Shared scratch storage

The home directory may be sufficient for most tasks. However, if your
job will produce many files and/or large files, then we recommend using
the scratch filesystems offered on Cannon. The scratch filesystem is
ideal for I/O intensive jobs. The Jacob Group has shared scratch storage
located in:

``` 
   $SCRATCH/jacob_lab/
```

where `$SCRATCH` is a system-defined environment variable that points to
the root scratch disk directory. (As of Mar 2020, `$SCRATCH` points to
`/n/holyscratch01`, but this could change in the future.) We recommend
that you create the subdirectory `$SCRATCH/jacob_lab/YOUR-USER-NAME` in
which to run your simulations and/or do your data analysis.

There is a 90 day time limit for files on the `$SCRATCH` disk space and
a 50 TB limit per group. However, if the `$SCRATCH` disk starts to
approach its storage capacity, Research Computing will begin deleting
files that are older than 90 days. This space is not backed up.

### Storage space

The Jacob Group has long-term storage space in:

``` 
  /n/seasasfs02/
```

Each Jacob Group user has a folder in that location with a quota of 500
GB. There is also a `share` folder which can be used to share files with
other group members. This space is backed up.

## Dedicated partition

Jacob-Group users can submit jobs to the huce\_intel partition on
Cannon. See [The HUCE Climate
Partitions](/wiki/as/slurm#the_huce_climate_partitions) wiki post for
more information. For a list of other available partitions, see [this
wiki post](/wiki/as/slurm#other_available_partitions).

When you log into Cannon, you will be placed into a login node. The
login nodes are sufficient for light computation, but for more
CPU-intensive tasks (e.g. running GEOS-Chem in interactive mode,
compiling with more than one processor, running IDL scripts), you should
request an interactive job. To request an interactive job on the
`huce_intel` partition, type:

``` 
   srun -p huce_intel --pty --x11=first --mem-per-cpu=<MB> -n 4 -N 1 /bin/bash
```

Large jobs should be submitted to SLURM using a batch script. See [this
wiki section](/wiki/as/slurm#set_up_a_run_script) for an example script
that can be used to submit a GEOS-Chem simulation to SLURM. To ensure
your job runs on the `huce_intel` partition, make sure you have the
following line in your script:

``` 
   #SBATCH -p huce_intel
```

To see a list of jobs currently submitted to the `huce_intel` partition,
type the following command:

``` 
   squeue -p huce_intel
```

The `huce_intel` partition has sufficient resources to support standard
GEOS-Chem simulations (aka "GC Classic")as well as large
[GCHP](http://wiki.seas.harvard.edu/geos-chem/index.php/GEOS-Chem_HP)
jobs that require hundreds of cores.

Odyssey uses a number of factors for job scheduling. Job priority is
assigned based on partition priority,
[fair-share](http://slurm.schedmd.com/priority_multifactor2.html#fairshare),
and length of time a job has been sitting in the queue. Cannon also
utilizes backfilling, which allows for small jobs to run on idle
resources until a high priority job requests those resources.

## Shared data directories

The GEOS-Chem data directories are located at:

``` 
  /n/holylfs/EXTERNAL_REPOS/GEOS-CHEM/gcgrid/data/ExtData
```

If you are creating a run directory from the Unit Tester, you will need
to modify `CopyRunDirs.input` so that you have:

``` 
  DATA_ROOT   : /n/holylfs/EXTERNAL_REPOS/GEOS-CHEM/gcgrid/data/ExtData
```

Otherwise, you will need to manually edit your `input.geos` and
`HEMCO_Config.rc` files to utilize this file path.

In input.geos:

``` 
  Root data directory     : /n/holylfs/EXTERNAL_REPOS/GEOS-CHEM/gcgrid/data/ExtData
```

In HEMCO\_Config.rc:

``` 
   ROOT:                        /n/holylfs/EXTERNAL_REPOS/GEOS-CHEM/gcgrid/data/ExtData/HEMCO
```
