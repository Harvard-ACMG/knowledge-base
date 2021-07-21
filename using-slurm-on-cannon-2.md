# Using Slurm to submit jobs on Cannon (part 2)

## Submit jobs to the queue

Large jobs (e.g. GEOS-Chem simulations) should be submitted as a batch job.  This will run your job in non-interactive mode on one of the computational nodes.
using a script.

### Set up a run script

Each GEOS-Chem Classic or GCHP run directory that you create will contain sample run scripts that you can modify.  Here are some examples:

  - GEOS-Chem Classic run script: [geoschem.run](https://github.com/geoschem/geos-chem/blob/main/run/GCClassic/runScriptSamples/geoschem.run)

  - GCHP run script [GCHP run](https://github.com/geoschem/geos-chem/blob/main/run/GCHP/runScriptSamples/operational_examples/harvard_gcst/gchp.run)

### Submit a batch job

You can use the Slurm `sbatch` command to submit a batch job:

``` 
   sbatch geoschem.run
```

### Specify job dependencies

Some jobs may need to be split up into a series of several jobs. To run a set of jobs in a specified order, you will need to make each job dependent on another job so that it won't execute until the earlier job has finished. You can specify job dependencies in SLURM using the `--dependency` option. For example:

``` 
  sbatch geoschem.run.1
  sbatch --dependency=afterok:JOBID geoschem.run.2
```

The dependency type `afterok` means that the second job will only begin after the specified `JOBID` has successfully executed.

### Check when your job will start

Adding this tag

    #SBATCH --test-only

at the top of your GEOS-Chem run script will invoke SLURM's "dry-run" option. SLURM will not schedule your job, but will instead return its interpretation of the requested resources and an example time for when it would be scheduled to run based on the current queue load. Here is a sample output:

    sbatch: Job 40513378 to start at 2018-03-31T15:16:26 using 24 processors on holyjacob01

This option can be very useful in helping to determine your workflow. If your job is not scheduled to start for a long time, you might want to consider starting other jobs that request less memory, cores, and/or time first.

For more information, also see: <https://docs.rc.fas.harvard.edu/kb/running-jobs/>

### Cancel a job

To kill a job that you've submitted, use the `scancel` command:

``` 
   scancel JOBID
```
