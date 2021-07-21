# Using Slurm to submit jobs on Cannon (part 4)

## Request enough time and memory

SLURM requires accurate information about the resources your job will require so that it can effectively schedule jobs. Therefore, you will want to provide a best estimate for the amount of time and memory required for your simulation. Requesting too much will adversely affect your job's priority in the queue. On the other hand, insufficient time or memory will cause your job to crash abruptly.

FASRC Research Computing recommends over-asking for your first job and then adjusting the time and memory requests for all subsequent jobs. The `sacct` command [described above](#sacct) can provide you with that information (look for `Elapsed` and `MaxRSS` in the output). 

To run GEOS-Chem in an interactive session or in the queue, all of the memory should be on the same node. That is why you need to use `-N 1` in the call to `srun` and in the `SBATCH` commands.

### Beware of requesting more time or memory than is available

Slurm will queue jobs that have requested resources that are unavailable. It doesn't distinguish between constraints due to usage and constraints due to the SLURM configuration. You can, for example, request more time than the maximum time limit for the partition and Slurm will happily queue your job even though it is unlikely to ever run.

If you requested that a job run for 10 days on the shared partition, the job would be queued, and the squeue command would show `PartitionTimeLimit` as the reason it is pending. To determine the partition time limit, you could run the command

``` 
scontrol show partition shared
```

and you'd see that the default time limit is 10 minutes and the maximum time limit is 7 days. In that case, you can cancel the job and resubmit using a smaller time limit.

As described above, you could also [use the Slurm dry-run option](#Check%20when%20your%20job%20will%20start) to ask SLURM when it thinks your job will begin executing, based on the current conditions.

## Check your fairshare score

If there are sufficient resources (i.e. cores and memory) on a given partition, then SLURM will typically start your job right away. But if all cores on the partition are currently in use, or there is not enough memory to run your job, then SLURM will cause your job to wait for a certain period before it starts.

SLURM computes a `fairshare` score (on a scale of 0 to 1), which is a factor that helps to determine how quickly a job will start. A fairshare is computed for each lab group, and all users in the group will have the same fairshare. Users having a higher fairshare can expect that their jobs will start running sooner than users with a lower fairshare. But even if a user has a low fairshare score, their job will be moved up in priority if the job has been pending for a very long time. So the priority at which a job starts is a combination of the user's fairshare score and the time it has been queued.

We recommend that you read FASRC's highly-comprehensive description of how the fairshare score is determined: <https://www.rc.fas.harvard.edu/fairshare/>

You can also use the following useful aliases to check your fairshare. Add these to your `~/.bash_aliases` or `~/.my_personal_settings` file:

`alias myfs="sshare -u <Username> -U"`
  - Display your [Fair-Share factor and other Slurm share information](https://slurm.schedmd.com/sshare.html) (replace `<Username>` with your username)

`alias jacobfs="sshare -A jacob_lab -a"`
