# Using Slurm to submit jobs on Cannon

The Cannon cluster uses the [Slurm](https://slurm.schedmd.com/) to manage its computational resources.  Here we provide a brief overview of how you can use Slurm to schedule jobs.

We also recommend that you read the [Running Jobs](https://docs.rc.fas.harvard.edu/kb/running-jobs/) page on the FASRC documentation site, which contains more detailed information about Slurm.

## Where will I run my jobs?

### The HUCE climate partitions

The Harvard University Center for the Enviroment has purchased several dedicated partitions on Cannon for exclusive use by Harvard atmospheric and climate modeling groups.  These [HUCE partitions](https://docs.rc.fas.harvard.edu/kb/huce-partitions/) have no time and memory limits, and are where you should run 

For most jobs, we recommmend the using `huce\_intel` partition, which consists of Intel Haswell and Broadwell CPUs. This should be fine for most purposes.

For more intensive modeling needs (or when `huce_intel` is busy), consider using the `huce\_cascade` partition.  This partition contains Intel Cascade Lake CPUs (which are newer and faster than the ones on `huce_intel`).

While it is possible to start interactive sessions in the `huce\_intel` or `huce_cascade` partitions, this will likely increase your [fairshare score](https://docs.rc.fas.harvard.edu/kb/fairshare/), which may cause your jobs to to take longer to start. For this reason, we recommend using the **test** partition for all interactive
sessions.

### Other available partitions

Cannon has other partitions [(described here in detail)](<https://docs.rc.fas.harvard.edu/kb/running-jobs/#Slurm_partitions>) that you can use.  However, your job will be competing for resources with users across the entire Cannon cluster.

## Useful SLURM commands

Before we go too much further, please take a moment to [review some of the more commonly used SLURM commands](<https://docs.rc.fas.harvard.edu/kb/convenient-slurm-commands/>)

## Requesting interactive jobs

When you log into Cannon, you will be placed into a login node.  The login nodes are sufficient for light computation, but for more CPU-intensive tasks (e.g. running GEOS-Chem in interactive mode, compiling with more than one processor, running IDL scripts), you should request an interactive job.

### Using the SLURM srun command

You can start an interactive session with the SLURM **srun**, which uses the syntax listed below. Text bracketed by \<\> denotes text typed by the user at the command line:

``` 
   srun -p <PARTITION> --pty --x11=first --mem=<MB> -c <NUMBER-OF-CORES> -N <NUMBER-OF-NODES> -t <TIME> --constraint=<CPUTYPE> -w <NODENAME> /bin/bash
```

where:

`-p <PARTITION-NAME>`
  - Requests a specific partition (aka queue) for the resource allocation. We recommend starting all interactive sessions in the Cannon **test** partition.                                                                               |
 
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

#### Example: Interactive session with 8 CPUs and load GNU compilers 10.2.0

``` 
srun -p test --pty --x11=first --mem=8000 -c 8 -n 1 /bin/bash
source ~/gcc_cmake.gfortran102_cannon.env
```

### Using the interactive\_openmp script

**NOTE: This is the recommended method of starting an interactive session\**

For your convenience, the GEOS-Chem Support Team has created a script called `interactive_openmp` that will issue [the SLURM `srun` command](#Using%20the%20SLURM%20srun%20command) with all of the proper options for you. You may download `interactive_openmp` from the [`cannon-env` repository of startup scripts](https://github.com/Harvard-ACMG/cannon-env). Make sure to copy `interactive_openmp` into your `~/bin` directory on Cannon.

The `interactive_openmp` script takes up to 4 arguments:

| \# | Argument                                                                              | Type     | Units   |
| -- | ------------------------------------------------------------------------------------- | -------- | ------- |
| 1  | \# of cores for the interactive session                                               | Required | \-      |
| 2  | Total memory for the entire job                                                       | Required | MB      |
| 3  | Length of time for the interactive session to run                                     | Required | minutes |
| 4  | Name of the SLURM partition (default = `test`) | Optional | \-      |

The first 3 arguments are mandatory. If you omit the 4th argument, the
interactive session will run on the `test` partition.

Here are some examples of how you can use `interactive_openmp` to start
interactive sessions:

``` 
   interactive_openmp 4 8000 120           # Start an interactive session w/ 4 cores, 8GB total memory, and 2 hours (assumes "test" partition)
   awake.sh &                              # Prevent interactive session from freezing
   load_if11                               # Load modules for ifort 11 compiler
   
   interactive_openmp 8 12000 480 test     # Start an interactive session w/ 8 cores, 12GB total memory, and 8 hours time (explicitly on "test")
   awake.sh &                              # Prevent interactive session from freezing
   load_gf71                               # Load modules for gfortran 7.1.0 compiler
```

The `awake.sh` script will be [described in more detail
below](#Problem%20with%20Odyssey%20interactive%20sessions%20freezing%20up).
The `$OMP_NUM_THREADS` variable for OpenMP will be set automatically for
you when you load the modules by typing `load_if11`, `load_gf71`, etc.

\--- *GEOS-Chem Support Team 2018/02/26 20:13*

### Make sure to set OMP\_NUM\_THREADS properly

Note that SLURM only requests a number of CPUs from the system, but it
will not actually tell GEOS-Chem how many cores to use. Parallelized
GEOS-Chem simulations will use the number of cores specified by the
environment variable `$OMP_NUM_THREADS`.

**$OMP\_NUM\_THREADS' is set for you automatically when you load modules
by typing e.g. load\_if11, load\_gf71, etc. This will set
$OMP\_NUM\_THREADS to the same number of CPUs that you requested in your
interactive session.**

If for some reason you wanted to change the value of $OMP\_NUM\_THREADS
within an interactive session, simply type:

    export OMP_NUM_THREADS=<NUMBER-OF_CORES>

where `<NUMBER-OF-CORES>` is the new number of cores that you want to
use.

### Problem with Cannon interactive sessions freezing up

**IMPORTANT NOTE\!** As of October 2015, interactive sessions on Cannon
(which was renamed from Odyssey in September 2019) tend to freeze if you
do not type anything at the Unix prompt for an extended length of time.
For example, if you leave your interactive session to go have lunch, and
come back an hour later, then you may find that the window in which your
interactive session was running does not accept any keystrokes. If this
happens to you, then you have to kill the window in which the
interactive job was running, open a new terminal window on Odyssey, and
launch a new interactive session.

To prevent this situation from occurring, the GEOS-Chem Support Team has
created a script called `awake.sh` that you can run in the background of
your interactive session. The `awake.sh` will print a `""` character to
the screen every 10 minutes. This is sufficient to prevent your
interactive jobs from freezing up.

You can obtain the `awake.sh` script from our [our repository of system
startup scripts](/wiki/as/startup_scripts). Make sure to copy it to your
`~/bin` directory.

As of October 2015, FAS RC is aware of this issue. RC thinks that it may
be a network error instead of a SLURM error. They are currently
investigating.

\--- *GEOS-Chem Support Team 2015/11/10 21:37*

## Submit jobs to the queue

Large jobs (e.g. GEOS-Chem simulations) should be submitted to SLURM
using a script.

### Set up a run script

Below is an example run script that you can customize for GEOS-Chem
jobs. This script may also be found in the [startup scripts Git
repository](/wiki/as/startup_scripts). You can rename the file and
modify its contents for your own simulation.

The `#SBATCH` lines in this script set key parameters for the SLURM
scheduler. See the script's ProTeX header or RC's [running jobs
page](https://rc.fas.harvard.edu/resources/running-jobs/#Submitting_batch_jobs_using_the_sbatch_command)
for more information about these options.

Example script:

    #!/bin/bash
    
    #SBATCH -c 8
    #SBATCH -N 1
    #SBATCH -t 0-12:00
    #SBATCH -p huce_intel
    #SBATCH --mem=8000
    #SBATCH --mail-type=END
    #SBATCH --no-requeue
    
    #------------------------------------------------------------------------------
    #                  GEOS-Chem Global Chemical Transport Model                  !
    #------------------------------------------------------------------------------
    #BOP
    #
    # !MODULE: run_geos_chem
    # 
    # !DESCRIPTION: Runs a GEOS-Chem "classic" simulation on Cannon.
    #\\
    #\\
    # !REMARKS:
    #  This script submits a typical 4x5 GEOS-Chem "classic" simulation 
    #  (i.e.using OpenMP serial parallelization) on Odyssey.  A 4x5 fullchem 
    #  simulation will take about 8 GB of RAM in total, across all CPUs.  (Your
    #  simulation may use more memory, especially if you are saving out a lot
    #  of diagnostic output.)  
    #
    #  The #SBATCH tags at the top of this script direct SLURM to reserve system
    #  resources for your job.  Here is a list of commonly-used #SBATCH options:
    #
    #  -c 8                   : Reserve 8 CPUs for the job.
    #  -N 1                   : Put all 8 CPUs on the same node.
    #  -t 0-12:00             : Ask for 12 hours of CPU time (format = DD-HH:MM)
    #  -p jacob               : Run job in the "jacob" queue.
    #  --constraint=intel     : Only run on nodes with Intel CPUs (any make)
    #  --constraint=haswell   : Only run on nodes with Intel Haswell CPUs
    #  --constraint=broadwell : Only run on nodes with Intel Broadwell CPUs
    #  --mem=8000             : Ask for 8 GB of total memory for the job.
    #  --mail-type=ALL        : Get notifications when the job starts/ends/fails.
    #  --mail-type=END        : Get notifications when the job ends.
    #  --mail-user=___        : Email address where notifications will get sent
    #  -o stdout.%j           : Send stdout to this log file. 
    #  -e stdout.%j           : Send stderr to this log file.
    #  --no-requeue           : Force SLURM to not to resubmit this job if a node 
    #                           fails and then reboots.  It is better to start the
    #                           GEOS-Chem job again from the last restart file.
    #
    #  Alternative options for SBATCH tags:
    #  (1) Instead of -c 8       you can also use --cpus-per-task=8
    #  (2) Instead of -N 1       you can also use --nodes=1
    #  (3) Instead of -t 0-12:00 you can also use -t 720 or -t 12:00
    #
    #  The most commonly-used partitions (i.e. run queues) on Odyssey are:
    #  (1) huce_intel     : Most GEOS-Chem "Classic" and GCHP jobs can run here.
    #  (2) huce_bigmem    : Only for GCHP jobs requiring up to 1.5 TB of memory.
    #  (3) shared         : Similar to huce_intel, but open to all of Odyssey.
    #  (3) test           : For interactive sessions only.
    # 
    #  Notes about stdout and stderr logs:
    #  (1) If you omit -o, SLURM will send stdout to slurm-JOBID.out
    #  (2) If you omit -e, SLURM will send stderr to slurm-JOBID.out
    #  (3) Recommended: Use %j to have SLURM append the JOBID to the
    #      stdout or stderr log file!  This is useful for debugging.
    #  (4) If you omit --mail-user, SLURM will send email to the submitting user.
    #
    #  The available options for email notification (--mail-user) are:
    #  (1) BEGIN : When the job starts
    #  (2) END   : When the job finishes
    #  (3) FAIL  : If the job dies with an error
    #  (4) ALL   : (1), (2), and (3) combined
    #
    # !REVISION HISTORY: 
    #  Use the "gitk" browser to view the Git version history!
    #EOP
    #------------------------------------------------------------------------------
    #BOC
    
    # Load environment modules
    # This will load the IFORT11 compiler. If you use a different compiler, make
    # sure to load the proper modules. Examples of other bash scripts for loading
    # modules can be found in the ~/init/ directory.
    source ~/.bashrc
    source ~/init/init.gc-classic.ifort11
    
    # Set the proper # of threads for OpenMP
    # SLURM_CPUS_PER_TASK ensures this matches the number you set with -n above
    export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
    
    # Run GEOS_Chem.  The "time" command will return CPU and wall times.
    # Stdout and stderr will be directed to the log files specified above.
    # NOTE: The "srun -c $OMP_NUM_THREADS" will tell SLURM that only
    # the line where we run the GEOS_Chem executable should get multiple threads.
    srun -c $OMP_NUM_THREADS time ./geos.mp
    
    # Exit normally
    exit 0
    #EOC

**NOTES**:

1.  The number of CPUs that you request using the `-n` option must match
    the number that you set for `OMP_NUM_THREADS`. To avoid errors, we
    recommend always setting `OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK`.
2.  Make sure to keep all of the `#SBATCH` tags together, and place them
    at the very top of your script, before any other comments or Unix
    commands. 
    1.  If you put a Unix command in the middle of `#SBATCH` tags, then
        SLURM will interpret the Unix command as the start of the
        script, and will ignore any `#SBATCH` tags that follow.
3.  The `SBATCH` parameters in the example script are configured for a
    4x5 GEOS-Chem benchmark simulation. You will need to modify the time
    (`-t`) and memory (`--mem`) options for other resolutions and
    simulations types. See the [request enough time and memory
    section](/[[wiki/as/slurm#request_enough_time_and_memory) for more
    information.
4.  You can use `--mem` to set the total amount of memory for the
    simulation, or `--mem-per-cpu` to set the amount of memory per each
    CPU. For jobs that use OpenMP (i.e. serial) parallelization, FAS
    Research Computing recommends the use of `--mem` instead of
    `--mem-per-cpu`.
5.  GEOS-Chem does not scale well on AMD CPUs. Therefore you should
    always use one of the following \#SBATCH tags to request purely
    Intel nodes:
    1.  `#SBATCH --constraint=intel`
    2.  `#SBATCH --constraint=haswell`
    3.  `#SBATCH --constraint=broadwell`

\--- *GEOS-Chem Support Team 2018/02/26 21:34*

### Submit a job

You can use the `sbatch` command to submit a job to SLURM. Using the
above run script called `run_geos_chem` as an example, type:

``` 
   sbatch run_geos_chem
```

\--- *GEOS-Chem Support Team 2015/10/29 19:05*

### Specify job dependencies

Some jobs may need to be split up into a series of several jobs. To run
a set of jobs in a specified order, you will need to make each job
dependent on another job so that it won't execute until the earlier job
has finished. You can specify job dependencies in SLURM using the
`--dependency` option. For example:

``` 
  sbatch run_geos_chem_1
  sbatch --dependency=afterok:JOBID run_geos_chem_2
```

The dependency type `afterok` means that the second job will only begin
after the specified `JOBID` has successfully executed.

#### Using the job\_depend.pl script

The GEOS-Chem Support Team has created for your convenience a Perl
script called `job_depend.pl`, which is available in the `bash/` folder
of [our repository of startup
scripts](/wiki/as/startup_scripts#repository_of_startup_scripts). This
script will submit several run scripts to the SLURM scheduler, with the
proper job dependencies option described above. This saves you from
having to manually issue these commands.

Copy `job_depend.pl` to your GEOS-Chem run directory. Then open it in
your favorite text editor and look for the lines:

    my @jobs   = qw/run_geos_chem.01
                    run_geos_chem.02
                    run_geos_chem.03/;

Then replace the existing text with the names of the GEOS-Chem run
scripts that you would like to submit in sequence. Then type at the
command line:

    ./job_depend.pl

which will submit the jobs. The second run script will not start
executing until the first run script has completed, and so forth.

\--- *GEOS-Chem Support Team 2018/03/28 19:10*

### Check when your job will start

Adding this tag

    #SBATCH --test-only

at the top of your GEOS-Chem run script will invoke SLURM's "dry-run"
option. SLURM will not schedule your job, but will instead return its
interpretation of the requested resources and an example time for when
it would be scheduled to run based on the current queue load. Here is a
sample output:

    sbatch: Job 40513378 to start at 2018-03-31T15:16:26 using 24 processors on holyjacob01

This option can be very useful in helping to determine your workflow. If
your job is not scheduled to start for a long time, you might want to
consider starting other jobs that request less memory, cores, and/or
time first.

For more information, also see:
<https://www.rc.fas.harvard.edu/resources/running-jobs/>

\--- *GEOS-Chem Support Team 2018/03/28 19:22*

### Cancel a job

To kill a job that you've submitted using `sbatch` you can use the
`scancel` command:

``` 
   scancel JOBID
```

\--- *GEOS-Chem Support Team 2015/10/29 19:05*

## Check the status of jobs

There are several ways in which you can get information about running or
completed SLURM jobs:

### The SLURM squeue command

The SLURM `squeue` command will let you check the status of all of your
current jobs (both interactive and queued).

**NOTE: FAS Research Computing (RC) recommends that you restrict the
`squeue` search (as described below), or else it will have to sift
through all of the running jobs. This can have a negative impact on
performance.**

You can look for all jobs being run by a single user by typing:

``` 
   squeue -u USERNAME
```

You can also check the status of all jobs running in a particular
partition using the `-p` option. For example to check the status jobs
running on the `jacob` partition, type:

``` 
   squeue -p jacob
```

\--- *GEOS-Chem Support Team 2015/10/29 19:07*

### The SLURM scontrol command

If your SLURM job is currently running, you can use the `scontrol`
command to get the SLURM configuration and job state. Typing a command
such as:

``` 
   scontrol show job 50499419
```

returns the following output:

    JobId=50499419 JobName=job.v11-01d
       UserId=ryantosca(556074) GroupId=jacob_lab(40114)
       Priority=116581311 Nice=0 Account=jacob_lab QOS=normal
       JobState=RUNNING Reason=None Dependency=(null)
       Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
       RunTime=19:21:14 TimeLimit=1-02:00:00 TimeMin=N/A
       SubmitTime=2015-10-28T16:12:07 EligibleTime=2015-10-28T16:12:07
       StartTime=2015-10-28T16:12:38 EndTime=2015-10-29T18:12:41
       PreemptTime=None SuspendTime=None SecsPreSuspend=0
       Partition=jacob AllocNode:Sid=rclogin12:27575
       ReqNodeList=(null) ExcNodeList=(null)
       NodeList=holyseas03
       BatchHost=holyseas03
       NumNodes=1 NumCPUs=8 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
       Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
       MinCPUsNode=1 MinMemoryCPU=2200M MinTmpDiskNode=0
       Features=(null) Gres=(null) Reservation=(null)
       Shared=OK Contiguous=0 Licenses=(null) Network=(null)
       Command=/n/home09/ryantosca/regal/UT/jobs/job.v11-01d
       WorkDir=/n/regal/jacob_lab/ryantosca/UT/logs/v11-01d
       StdErr=/n/home09/ryantosca/regal/UT/logs/v11-01d/v11-01d.stderr.log
       StdIn=/dev/null
       StdOut=/n/home09/ryantosca/regal/UT/logs/v11-01d/v11-01d.stdout.log

NOTE: This information will be purged from memory 5 minutes after your
job finishes. But you can use either `sacct`, `jobinfo` (recommended\!)
or `jobstats` to view the results of a completed SLURM job as described
below.

\--- *GEOS-Chem Support Team 2015/10/29 18:00*

### The SLURM sacct command

If you know the SLURM job ID number, you can use the `sacct` command to
get information for a current or past SLURM job. Typing

``` 
   sacct -l -j JOBID
```

will return the "long" output information (specified with `-l`). Note
that the default output format contains many columns, which may get
wrapped around on your screen. For example, here is output from an
actual SLURM job:

``` 
  sacct -l -j 50499419

       JobID     JobIDRaw    JobName  Partition  MaxVMSize  MaxVMSizeNode  MaxVMSizeTask  AveVMSize     MaxRSS MaxRSSNode MaxRSSTask     AveRSS MaxPages
 MaxPagesNode   MaxPagesTask   AvePages     MinCPU MinCPUNode MinCPUTask     AveCPU   NTasks  AllocCPUS    Elapsed      State ExitCode AveCPUFreq ReqCPUFreq
     ReqMem ConsumedEnergy  MaxDiskRead MaxDiskReadNode MaxDiskReadTask    AveDiskRead MaxDiskWrite MaxDiskWriteNode MaxDiskWriteTask   AveDiskWrite
    AllocGRES      ReqGRES 
------------ ------------ ---------- ---------- ---------- -------------- -------------- ---------- ---------- ---------- ---------- ---------- --------
 ------------ -------------- ---------- ---------- ---------- ---------- ---------- -------- ---------- ---------- ---------- -------- ---------- ----------
 ---------- -------------- ------------ --------------- --------------- -------------- ------------ ---------------- ---------------- -------------- ------------
 ------------ 
50499419     50499419     job.v11-0+      jacob
  
                                                                                                                              8   19:03:17
RUNNING      0:0               Unknown     2200Mc                 
```

You may also specify which information you would like to obtain for your
job with the `--format` keyword. You can select only the most relevant
fields:

``` 
   sacct -j 50499419 --format=JobID,JobName,User,Partition,NNodes,NCPUS,MaxRSS,CPUTimeRAW,Elapsed
```

which returns this shortened output:

``` 
       JobID    JobName      User  Partition   NNodes      NCPUS     MaxRSS CPUTimeRAW    Elapsed 
------------ ---------- --------- ---------- -------- ---------- ---------- ---------- ---------- 
50499419     job.v11-0+ ryantosca      jacob        1          8                552864   19:11:48 
```

For more information on the job accounting fields for the `--format`
option, see <http://slurm.schedmd.com/sacct.html>.

\--- *GEOS-Chem Support Team 2015/10/29 18:07*

### The jobinfo script

**This is the recommended method for getting information about SLURM
jobs\!**

For your convenience, the GEOS-Chem Support Team has created the
`jobinfo` script. This script calls `sacct -l -j` ([as described in the
preceding section](#The%20SLURM%20sacct%20command)), and then parses the
output into an easy-to-read format. All non-blank fields from `sacct`
output will be displayed.

You can obtain the `jobinfo` script from [our repository of system
startup scripts](/wiki/as/startup_scripts). Make sure to place it into
your `~/bin` directory.

To check get information of a job, simply type:

``` 
  jobinfo JOBID
```

If the job is running, your output will look similar to this:

``` 
  jobinfo 50618631

  JobName          : job.v11-01d           
  Submit           : 2015-10-30 10:38:48   
  Start            : 2015-10-30 10:38:51   
  End              : Unknown               
  JobID            : 50618631     
  JobIDRaw         : 50618631
  JobName          : job.v11-01d
  Partition        : jacob                 
  AllocCPUS        : 8                     
  Elapsed          : 02:14:31              
  State            : RUNNING               
  ExitCode         : 0:0                   
  ReqCPUFreq       : Unknown               
  ReqMem           : 3 GB/cpu              
  TotalReqMem      : 24 GB
```

If the job has finished, the output will contain additional information,
including the amount of memory and CPU time that was actually used:

``` 
 jobinfo 51519789

 JobName          : run_geos_chem         batch                 
 Submit           : 2015-11-10 14:44:23   2015-11-10 14:44:24   
 Start            : 2015-11-10 14:44:24   2015-11-10 14:44:24   
 End              : 2015-11-10 15:02:08   2015-11-10 15:02:08   
 UserCPU          : 01:53:14              01:53:14              
 TotalCPU         : 01:54:28              01:54:28              
 JobID            : 51519789              51519789.ba+          
 JobIDRaw         : 51519789              51519789.ba+          
 JobName          : run_geos_chem         batch                 
 Partition        : jacob                                       
 MaxVMSize        :                       9.4849 GB             
 MaxVMSizeNode    :                       holyseas03            
 MaxVMSizeTask    :                       0                     
 AveVMSize        :                       9.4849 GB             
 MaxRSS           :                       5.5698 GB             
 MaxRSSNode       :                       holyseas03            
 MaxRSSTask       :                       0                     
 AveRSS           :                       5.5698 GB             
 MaxPages         :                       0.0007 GB             
 MaxPagesNode     :                       holyseas03            
 MaxPagesTask     :                       0                     
 AvePages         :                       0.0007 GB             
 MinCPU           :                       01:44:55              
 MinCPUNode       :                       holyseas03            
 MinCPUTask       :                       0                     
 AveCPU           :                       01:44:55              
 NTasks           :                       1                     
 AllocCPUS        : 8                     8                     
 Elapsed          : 00:17:44              00:17:44              
 State            : COMPLETED             COMPLETED             
 ExitCode         : 0:0                   0:0                   
 AveCPUFreq       :                       0.253 GHz             
 ReqCPUFreq       : Unknown               0                     
 ReqMem           : 8 GB/node             8 GB/node             
 ConsumedEnergy   :                       0                     
 MaxDiskRead      :                       3.233 GB              
 MaxDiskReadNode  :                       holyseas03            
 MaxDiskReadTask  :                       0                     
 AveDiskRead      :                       3.233 GB              
 MaxDiskWrite     :                       0.001 GB              
 MaxDiskWriteNode :                       holyseas03            
 MaxDiskWriteTask :                       0                     
 AveDiskWrite     :                       0.001 GB              
 TotalReqMem      : 8 GB                  8 GB
```

The job is represented in two columns. The first column shows the
partition (aka run queue) to which the job was submitted (in this case,
`jacob`). The second column contains statistics for the actual node that
the job ran on (in this case, `holyseas03`). Also note that `jobinfo`
reports all memory values in GB, so you don't have to do the unit
conversion from MB or KB in your head.

The `JobName` field---which displays the name of the script that SLURM
is executing---will be displayed with a default width of 20 characters.
You can specify a different width for `JobName` by passing a second
argument to `jobinfo`. For example:

``` 
 jobinfo 51519789 30
```

will display `JobName` with a width of 30 characters.

\--- *GEOS-Chem Support Team 2015/11/10 21:04*

### The jobstats script

For your convenience, the GEOS-Chem Support Team has created a script
called `jobstats` that displays the most important job statistics. You
can obtain the `jobstats` script from [our repository of system startup
scripts](/wiki/as/startup_scripts). Make sure to place it into your
`~/bin` directory.

We'll use the same JOBID from the previous section. If you type:

``` 
 jobstats 51519789
```

you will get the following output:

``` 
 SLURM JobID #         : 51519789
 Job Name              : run_geos_chem
 Submit time           : 2015-11-10 14:44:23
 Start  time           : 2015-11-10 14:44:24
 End    time           : 2015-11-10 15:02:08
 Partition             : jacob
 Node                  : holyseas03
 CPUs                  : 8
 Memory                : 5.5698 GB
 CPU  Time             : 01:54:28    (       6868 s)
 Wall Time             : 00:17:44    (       1064 s)
 CPU  Time / Wall Time : 6.4549      ( 80.69% ideal)
```

The quantity `CPU Time / Wall Time` is the ratio of the SLURM output
fields `TotalCPU / Elapsed`. This ratio is an approximate measure of how
efficiently your job has been parallelized.

Because this job ran on 8 CPUs, then the ideal `CPU Time / Wall Time`
ratio would have been 8. But due to computational overhead and file I/O
operations, it actually has a `CPU Time / Wall Time` ratio of 6.4549.
This means that the job actually ran at about 81% ( = 6.4549 / 8 \* 100%
) of ideal performance.

**NOTE:** The above example is from a [GEOS-Chem Unit
Test](http://wiki.seas.harvard.edu/geos-chem/index.php/Debugging_with_the_GEOS-Chem_unit_tester)
1-day "benchmark" simulation. The first day of a GEOS-Chem simuation is
where much of the I/O occurs; therefore, simulations that run for longer
periods of time will probably have a higher `CPU Time / Wall Time` ratio
than the example shown above.

**ALSO NOTE:** SLURM only counts the number of cores requested, but not
the number of cores that your job actually used. In many cases (e.g. for
timing tests or benchmarking), yow will want to request all of the cores
on a node but only use some of them. (This prevents other people's jobs
from "backfilling" onto the node your job is running on, which can
affect timing results.) So if you requested all of the cores on the
node, but your job only used 8 cores, then you will get a more accurate
output for the "% ideal" parameter if you tell the `jobstats` script
that you used 8 cores. You specify this by with the second argument,
e.g.:

    jobstats 51519789 8

\--- *GEOS-Chem Support Team 2015/11/10 21:12*

### Glossary of SLURM output fields

When you use the `sacct`, `scontrol`, `jobinfo`, or `jobstats` to get
more information about a current or past SLURM job, you will be
presented with several output fields. The most important fields are
listed in the table below.

A note about units:

  - If you use `sacct` or `scontrol`, then MEMORY INFO fields will be
    displayed in either KB or MB (depending on the quantity).
  - If you use `jobinfo` or `jobstats`, all MEMORY INFO fields will be
    converted to GB, for consistency's sake.

| JOB INFO           | Description                                                                                                                                                                                  |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `JobName`          | Name of the script that was submitted to SLURM.                                                                                                                                              |
| `JobID`            | The Job ID number. This is returned by the SLURM `sbatch` command (for queued jobs) or by the SLURM `srun` command (for interactive jobs).                                                   |
| `Submit`           | Date and time when the job was submitted to SLURM. (Format is `YYYY-MM-DD hh:mm:ss`.)                                                                                                        |
| `Start`            | Date and time when the job started running. (Uses same format as `Submit`.)                                                                                                                  |
| `End`              | Date and time when the job stopped running. (Uses same format as `Submit`.)                                                                                                                  |
| `Partition`        | Name of the "queue" in which the job ran. This will usually be `jacob`, `general`, or `interact`.                                                                                            |
| `NTasks`           | Number of tasks. For GEOS-Chem "classic" simulations using OpenMP, this will always equal 1.                                                                                                 |
| `AllocCPUS`        | Number of CPUs requested by the user.                                                                                                                                                        |
| `Status`           | Current job status (e.g. `CANCELLED`, `COMPLETED`, `FAILED`, `PENDING`, `RUNNING`, `SUSPENDED`, `TIMEOUT`, etc.)                                                                             |
| `ExitCode`         | Exit code issued by the job. An exit code of 0 means the job finished sucessfully. A non-zero exit code means that the job was terminated abnormally.                                        |
| TIMING INFO        | Description                                                                                                                                                                                  |
| `UserCPU`          | The amount of user CPU time used by the job. (Format is `DD-hh:mm:ss`, `hh:mm:ss`, or `mm:ss`, depending on how long the job ran.)                                                           |
| `SystemCPU`        | The amount of system CPU time used by the job. (Uses same format as `UserCPU`.)                                                                                                              |
| `TotalCPU`         | The total amount of CPU time: `TotalCPU = UserCPU + System CPU`. (Uses same format as `UserCPU`.) The `jobstats` script uses the `TotalCPU` field to compute the ratio `CPU Time/Wall Time`. |
| `Elapsed`          | The total amount of "wall clock" time used by the job. (Uses same format as `UserCPU`.) The `jobstats` script uses the `Elapsed` field to compute the ratio `CPU Time/Wall Time`.            |
| DISK I/O INFO      | DESCRIPTION                                                                                                                                                                                  |
| `MaxDiskRead`      | Maximum amount of data read from disk by all tasks in a job.                                                                                                                                 |
| `MaxDiskReadNode`  | Node on which the maximum disk read (`MaxDiskReadNode`) occurred.                                                                                                                            |
| `MaxDiskReadTask`  | The task ID for which the maximum disk read (`MaxDiskReadNode`) occurred. NOTE: GEOS-Chem "classic" jobs always run as a single task, so the task ID will always be 0.                       |
| `MaxDiskWrite`     | Maximum amount of data written to disk by all tasks in a job.                                                                                                                                |
| `MaxDiskWriteNode` | Node on which the maximum disk write (`MaxDiskWriteNode`) occurred.                                                                                                                          |
| `MaxDiskWriteTask` | The task ID for which the maximum disk read (`MaxDiskWriteNode`) occurred. NOTE: GEOS-Chem "classic" jobs always run as a single task, so the task ID will always be 0.                      |
| MEMORY INFO        | DESCRIPTION                                                                                                                                                                                  |
| `MaxRSS`           | Maximum amount of onboard memory (aka "resident set size") used by the job.                                                                                                                  |
| `MaxRSSNode`       | Node on which the maximum amount of onboard memory (`MaxRSS`) occurred.                                                                                                                      |
| `MaxRSSTask`       | The task ID for which the maximum amount onboard mamory (`MaxRSS`) occurred. NOTE: GEOS-Chem "classic" jobs always run as a single task, so the task ID will always be 0.                    |
| `MaxVMSize`        | Maximum amount of virtual memory (VM) used by the job. Virtual memory = resident onboard memory + swap space memory.                                                                         |
| `MaxVMSizeNode`    | Node on which the maximum amount of VM usage (`MaxVmSize`) occurred.                                                                                                                         |
| `MaxVMSizeTask`    | The task ID for which the maximum amount VM usage (`MaxVMSize`) occurred. GEOS-Chem "classic" jobs always run as a single task, so the task ID will always be 0.                             |
| `ReqMem`           | The amount of memory requested by the user per CPU. This is the value specified by the `--mem-per-cpu` option.                                                                               |
| `TotalReqMem`      | This field is returned by the `jobinfo` script. It is the total amount of requested memory (i.e. `ReqMem` multiplied by `AllocCPUS`).                                                        |

\--- *GEOS-Chem Support Team 2015/11/03 17:14*

## Request enough time and memory

SLURM requires accurate information about the resources your job will
require so that it can effectively schedule jobs. Therefore, you will
want to provide a best estimate for the amount of time and memory
required for your simulation. Requesting too much will adversely affect
your job's priority in the queue. On the other hand, insufficient time
or memory will cause your job to crash abruptly.

A typical GEOS-Chem v10-01 4x5 benchmark simulation takes about 6.5
hours and 6-7 GB to run. You can run a test simulation to verify those
statistics or to determine the time and memory requirements for other
resolutions and simulation types. Research Computing recommends
over-asking for your first job and then adjusting the time and memory
requests for all subsequent jobs. The `sacct` command [described
above](/wiki/as/slurm#get_detailed_job_information) can provide you with
that information (look for `Elapsed` and `MaxRSS` in the output).

To run GEOS-Chem in an interactive session or in the queue, all of the
memory should be on the same node. That is why you need to use `-N 1` in
the call to `srun` and in the `SBATCH` commands.

### Beware of requesting more time or memory than is available

SLURM will queue jobs that have requested resources that are
unavailable. It doesn't distinguish between constraints due to usage and
constraints due to the SLURM configuration. You can, for example,
request more time than the maximum time limit for the partition and
SLURM will happily queue your job even though it is unlikely to ever
run.

If you requested that a job run for 10 days on the shared partition, the
job would be queued, and the squeue command would show
`PartitionTimeLimit` as the reason it is pending. To determine the
partition time limit, you could run the command

``` 
   scontrol show partition shared
```

and you'd see that the default time limit is 10 minutes and the maximum
time limit is 7 days. In that case, you can cancel the job and resubmit
using a smaller time limit.

As described above, you could also [use the SLURM dry-run
option](#Check%20when%20your%20job%20will%20start) to ask SLURM when it
thinks your job will begin executing, based on the current conditions.

\--- *GEOS-Chem Support Team 2015/10/29 19:03*

## Check your fairshare score

If there are sufficient resources (i.e. cores and memory) on a given
partition, then SLURM will typically start your job right away. But if
all cores on the partition are currently in use, or there is not enough
memory to run your job, then SLURM will cause your job to wait for a
certain period before it starts.

SLURM computes a **fairshare** score (on a scale of 0 to 1), which is a
factor that helps to determine how quickly a job will start. A fairshare
is computed for each lab group, and all users in the group will have the
same fairshare. Users having a higher fairshare can expect that their
jobs will start running sooner than users with a lower fairshare. But
even if a user has a low fairshare score, his or her job will be moved
up in priority if the job has been pending for a very long time. So the
priority at which a job starts is a combination of the user's fairshare
score and the time it has been queued.

We recommend that you read FASRC's highly-comprehensive description of
how the fairshare score is determined:
<https://www.rc.fas.harvard.edu/fairshare/>

You can also use the following useful aliases to check your fairshare:

| Alias                                    | Description                                                                                                                                         |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `alias myfs="sshare -u <Username> -U"`   | Display your [Fair-Share factor and other SLURM share information](https://slurm.schedmd.com/sshare.html) (replace `<Username>` with your username) |
| `alias jacobfs="sshare -A jacob_lab -a"` | Display the Fair-Share factor for all users in jacob\_lab                                                                                           |

\--- *GEOS-Chem Support Team 2018/03/28 20:18*

## SLURM warning messages

The following SLURM warning messages are sometimes known to occur:

### slurmstepd error: Exceeded job memory limit

**Bob Yantosca wrote:**

> So I ran a GEOS-Chem job in the jacob partition (SLURM ID: 80576872).
> The job finished normally but in the log file output there was a
> slurmstepd error message:

\>

> real 9355.80
> 
> user 62167.67
> 
> sys 386.40
> 
> slurmstepd: error: Exceeded step memory limit at some point.
> 
> slurmstepd: error: Exceeded job memory limit at some point.
> 
> \# of CPUs : 8
> 
> NodeName : regal16.rc.fas.harvard.edu

\>

> The question is: should I be concerned about this? Does that mean that
> the job output is corrupted?I also should point out that other users
> in my group have seen the same error, when running different programs
> (e.g. MITgcm)

**Scott Yockel replied::**

> If you are getting that message and your job is fine, I wouldn't worry
> to much about it.
> 
> What happens when you setup a memory reservation in SLURM via --mem is
> that a Linux cgroup is created to keep you from going over that
> amount. There is a threshold of that amount that it creates that
> warning message. The data in sacct that slurmdb collects is only
> updated every 30 seconds. So it may be possible that your job has a
> few memory spikes beyond the MaxRSS that is triggering that warning
> message, but not going all the way above the cgroup limit that would
> cause it to kill the job. Also, you can track the MaxVMS to see the
> virtual memory space as well. Some codes, like MatLab have some odd
> behavior where it allocates the virtual memory at the beginning, which
> is X% larger than the MaxRSS (resident memory), and that can cause an
> issue with the cgroup amount. So we've had success having those user
> set mem = MaxRSS + MaxVMS with a little extra space added.

\>

> I hope that explains the behavior.

## More information about running jobs

Detailed information about running jobs on Odyssey can be found on the
Research Computing website at
<https://rc.fas.harvard.edu/resources/running-jobs/>.

\--- *GEOS-Chem Support Team 2015/10/29 13:35*

## Return to Menu

  - [Go to Harvard Atmospheric Sciences Wiki Home](/start)
  - [Go to "The Basics" Menu](/wiki/basics)
  - [Go to Network Resources Menu](/wiki/computer_and_network_info)
