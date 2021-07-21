# Using Slurm to submit jobs on Cannon (part 1)

The Cannon cluster uses the [Slurm](https://slurm.schedmd.com/) to manage its computational resources.  Here we provide a brief overview of how you can use Slurm to schedule jobs.

We also recommend that you read the [Running Jobs](https://docs.rc.fas.harvard.edu/kb/running-jobs/) page on the FASRC documentation site, which contains more detailed information about Slurm.

## Where will I run my jobs?

### The HUCE climate partitions

The Harvard University Center for the Enviroment has purchased several dedicated partitions on Cannon for exclusive use by Harvard atmospheric and climate modeling groups.  These [HUCE partitions](https://docs.rc.fas.harvard.edu/kb/huce-partitions/) have no time and memory limits, and are where you should run 

For most jobs, we recommmend the using `huce_intel` partition, which consists of Intel Haswell and Broadwell CPUs. This should be fine for most purposes.

For more intensive modeling needs (or when `huce_intel` is busy), consider using the `huce_cascade` partition.  This partition contains Intel Cascade Lake CPUs (which are newer and faster than the ones on `huce_intel`).

While it is possible to start interactive sessions in the `huce_intel` or `huce_cascade` partitions, this will likely increase your [fairshare score](https://docs.rc.fas.harvard.edu/kb/fairshare/), which may cause your jobs to to take longer to start. For this reason, we recommend using the `test` partition for all interactive sessions.

### Other available partitions

Cannon has other partitions [(described here in detail)](<https://docs.rc.fas.harvard.edu/kb/running-jobs/#Slurm_partitions>) that you can use.  However, your job will be competing for resources with users across the entire Cannon cluster.

## Useful SLURM commands

Before we go too much further, please take a moment to [review some of the more commonly used SLURM commands](<https://docs.rc.fas.harvard.edu/kb/convenient-slurm-commands/>)

## Requesting interactive jobs

When you log into Cannon, you will be placed into a login node.  The login nodes are sufficient for light computation, but for more CPU-intensive tasks (e.g. running GEOS-Chem in interactive mode, compiling with more than one processor, running IDL scripts), you should request an interactive job.

### Method 1: Requesting interactive jobs with the interactive\_openmp script

**NOTE: This is the recommended method of starting an interactive session**

For your convenience, the GEOS-Chem Support Team has created a script called `interactive_openmp` that will issue the Slurm `srun` command (see below) with all of the proper options for you. You may download `interactive_openmp` from the [`cannon-env` repository of startup scripts](https://github.com/Harvard-ACMG/cannon-env). Make sure to copy `interactive_openmp` into your `~/bin` directory on Cannon.

The `interactive_openmp` script takes up to 4 arguments:

| \# | Argument                                                                              | Type     | Units   |
| -- | ------------------------------------------------------------------------------------- | -------- | ------- |
| 1  | \# of cores for the interactive session                                               | Required | \-      |
| 2  | Total memory for the entire job                                                       | Required | MB      |
| 3  | Length of time for the interactive session to run                                     | Required | minutes |
| 4  | Name of the SLURM partition (default = `test`) | Optional | \-      |

The first 3 arguments are mandatory. If you omit the 4th argument, the
interactive session will run on the `test` partition.

Here are some examples of how you can use `interactive_openmp` to start interactive sessions:

#### Example: Interactive job (4 cores, 8GB memory, 2 hours)
``` 
interactive_openmp 4 8000 120                     
source ~/envs/gcc_cmake.gfortran102_cannon.env
```

#### Example: Interactive job (8 cores, 12GB memory, 8 hours)
```
interactive_openmp 8 12000 480 test
source ~/envs/gcc_cmake.gfortran102_cannon.env                               
```
### Method 2: Requesting interactive jobs with the Slurm srun command

You can start an interactive session with the SLURM `srun`, which uses the syntax listed below. Text bracketed by \<\> denotes text typed by the user at the command line:

``` 
   srun -p <PARTITION> --pty --x11=first --mem=<MB> -c <NUMBER-OF-CORES> -N <NUMBER-OF-NODES> -t <TIME> --constraint=<CPUTYPE> -w <NODENAME> /bin/bash
```

where:

`-p <PARTITION-NAME>`
  - Requests a specific partition (aka queue) for the resource allocation. We recommend starting all interactive sessions in the Cannon `test` partition.                                                                               |
 
`-pty`
  - Starts in pseudo-terminal mode. 

`--x11=first`
  - Starts X11 display (for graphical window display)

`-c <NUMBER-OF-CORES>`
  - Specifies the number of cores per node that your job will use.

`-N <NUMBER-OF-NODES>`
  - Requests the number of nodes that will be allocated to this job.
    - For GEOS-Chem "Classic" simulations, you can only use 1 node due to limitations of the OpenMP parallelization.
    - For GCHP simulations, you may use more than one node.

`--mem=<MB>`
  - Specifies the real memory required per node in MegaBytes.

`-t <TIME>`
  - Specifies the time limit for the interactive job in minutes. Acceptable formats for time are `minutes`, `hours:minutes:seconds`, and `days-hours:minutes`.

`--constraint=<CPUTYPE>`
  - (OPTIONAL) Requests that the interactive session only execute on nodes having a specific type of cpu. for example, if you only wanted to run on Intel CPUs, use `--constraint=intel`.

`-w <NODENAME>`
  - (OPTIONAL) requests that the interactive session execute on a specific node.

`--no-requeue`
  - (OPTIONAL) tells the slurm scheduler not to automatically re-submit your job when a node fails and reboots. It is always best to manually restart your geos-chem simulation using the most recently-created restart file.

`/bin/bash`
  - The unix shell (in this case, bash) that you want to use for the interactive session. in this case, because we are using bash, your `.bashrc` (and any alias files like `.bash_aliases` or `.my_personal_settings`) will be sourced.

After you request an interactive session, you may notice that your login prompt may change. For example, when you log into cannon using `login.rc.fas.harvard.edu`, your unix prompt may have looked like this:

``` 
   USER@holylogin04
```

But in the interactive session, your prompt may look something like this:

``` 
   USER@holyc19315
```

NOTE: if you are on the one of the `holy*` nodes on cannon, then this means you are on a machine in holyoke, ma (about 100 miles from Harvard).

#### Example: Interactive session (8 CPUs)

``` 
srun -p test --pty --x11=first --mem=8000 -c 8 -n 1 /bin/bash
source ~/envs/gcc_cmake.gfortran102_cannon.env
```

### Make sure to set OMP\_NUM\_THREADS properly

Note that SLURM only requests a number of CPUs from the system, but it will not actually tell GEOS-Chem how many cores to use. Parallelized GEOS-Chem simulations will use the number of cores specified by the environment variable `$OMP_NUM_THREADS`.

`$OMP_NUM_THREADS` will be set automatically for you when you source one of the [GEOS-Chem environment files](cannon-environment-files.md).  This will set `$OMP_NUM_THREADS` to the same number of CPUs that you requested in your
interactive session.

If for some reason you wanted to change the value of `$OMP_NUM_THREADS` within an interactive session, simply type:

    export OMP_NUM_THREADS=<NUMBER-OF_CORES>

where `<NUMBER-OF-CORES>` is the new number of cores that you want to use.

### Problem with Cannon interactive sessions freezing up

Cannon interactive sessions tend to freeze if you do not type anything at the Unix prompt for a long time.  To prevent this behavior, add this text to your `~/.ssh/config` file (or create it if it does not exist):
```
Host *
 ServerAliveCountMax 6
 ServerAliveInterval 240
```
This will cause a packet to be sent every 240 seconds = 4 minutes to the interactive job, which should be sufficient to keep it alive.
