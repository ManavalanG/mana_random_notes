- [SLURM](#slurm)
  - [Basic commands](#basic-commands)
    - [Submitting jobs](#submitting-jobs)
    - [Listing jobs](#listing-jobs)
      - [squeue](#squeue)
    - [Info on currently running jobs](#info-on-currently-running-jobs)
      - [scontrol](#scontrol)
      - [sstat](#sstat)
    - [Info on completed jobs](#info-on-completed-jobs)
      - [seff](#seff)
      - [sacct](#sacct)
    - [Controlling jobs](#controlling-jobs)
      - [scancel](#scancel)
      - [scontrol](#scontrol-1)
    - [Interactive jobs](#interactive-jobs)
      - [srun](#srun)

# SLURM

## Basic commands

Many info here are stolen from: https://www.rc.fas.harvard.edu/resources/documentation/convenient-slurm-commands/

### Submitting jobs

```sh
# submit jobs via script
sbatch myscript.sh

# to submit jobs directly without passing script
sbatch << EOF
#!/bin/sh

sleep 30
EOF
```

### Listing jobs

#### squeue

```sh
# list all current jobs for a user
squeue -u <username>

# List all running jobs for a user
squeue -u <username> -t RUNNING

# List all pending jobs for a user
squeue -u <username> -t PENDING


# squeue aliases I find useful
alias SQ='squeue -o "%.8i %.20j %.10P %.7u %.5D %.4C %.11M  %.11l %.3t %.11m %R" -u $USER'
alias SQ_long='squeue -o "%.8i %.20j %.10P %.7u %.5D %.4C %.11M  %.11l %.3t %.11m %R %V %o" -u $USER'  #also shows submission time and command ran

# Total no. of jobs
alias no_jobs="SQ -h | wc -l"
```

### Info on currently running jobs

#### scontrol

```sh
# List detailed information for a job (useful for troubleshooting)
scontrol show job -d <jobid>

# aliases I find useful
alias SCONTR='scontrol show job -d'
```

#### sstat

```sh
# List status info for a currently running job
sstat --format=JobID,AveCPU,AvePages,AveRSS,AveVMSize --allsteps -j <jobid>

# aliases I find useful
alias SR='sstat --format="JobID,NTasks,AveCPU,AvePages,AveRSS,AveVMSize,MaxRSSNode" --allsteps'
```


### Info on completed jobs

#### seff

```sh
# provides useful info on completed job, including the memory used and what percent of your allocated memory that amounts to.
seff <jobid>
```

#### sacct

```sh
# To view the same information for all jobs of a user
sacct --format=JobID,JobName,MaxRSS,Elapsed,state,ReqMem,MaxVMSize,AveVMSize --units=M

# To get statistics on completed jobs by jobID
sacct --format=JobID,JobName,MaxRSS,Elapsed,state,ReqMem,MaxVMSize,AveVMSize --units=M -j <jobid>

# shows resources allocated
sacct --allocations

# jobs between select dates; valid formats for day-time: HH:MM[:SS] [AM|PM], MMDD[YY] or MM/DD[/YY] or MM.DD[.YY], MM/DD[/YY]-HH:MM[:SS], YYYY-MM-DD[THH:MM[:SS]]
sacct --starttime 2020-04-17T14:00:00 --endtime 2020-04-25T23:59:59 --allocations
sacct -S $(date -d '1 month ago' +%D-%R) -E $(date -d '2 weeks ago' +%D-%R)

# sacct aliases I find useful
alias SC='sacct --format="JobID,JobName,Ntasks,MaxRSS,Elapsed,state,NodeList,ReqMem,MaxVMSize,AveVMSize,Partition,AllocTRES%40" --units=M'
```


##### Useful flags and their meaning

[Source](https://rc.byu.edu/wiki/?id=Using+sacct)

###### CPU

| Flag       | Meaning                                                            |
|------------|--------------------------------------------------------------------|
| NCPUs      | Number of CPUs used by the job                                     |
| NNodes     | Number of nodes used by the job                                    |
| UserCPU    | User CPU time used by the job                                      |
| SystemCPU  | System CPU time used by the job                                    |
| TotalCPU   | Total CPU time used by the job; sum of UserCPU and SystemCPU       |
| CPUTime    | Elapsed*NCPUs (total CPU time a perfectly efficient job would use) |

###### Memory

| Flag    | Meaning                                                                   |
|---------|---------------------------------------------------------------------------|
| ReqMem  | Amount of memory requested; suffixed with 'c' if per CPU, 'n' if per node |
| AveRSS  | Average memory use for all tasks                                          |
| MaxRSS  | Maximum memory use of any task                                            |

###### Time

| Flag       | Meaning                             |
|------------|-------------------------------------|
| Submit     | When the job was submitted          |
| Start      | When the job started                |
| End        | When the job ended                  |
| TimeLimit  | How much time the job was allocated |
| Elapsed    | How much time the job used          |

###### I/O

| Flag          | Meaning                                       |
|---------------|-----------------------------------------------|
| AveDiskRead   | Average number of bytes read for all tasks    |
| MaxDiskRead   | Maximum number of bytes read for any task     |
| AveDiskWrite  | Average number of bytes written for all tasks |
| MaxDiskWrite  | Maximum number of bytes read for any task     |
| AvePages      | Average number of page faults for all tasks   |
| MaxPages      | Maximum number of page faults for any task    |


### Controlling jobs

#### scancel

```sh
# To cancel one job
scancel <jobid>

# To cancel all the jobs for a user
scancel -u <username>

# To cancel all the pending jobs for a user
scancel -t PENDING -u <username>

# To cancel one or more jobs by name
scancel --name myJobName
```

#### scontrol

```sh
# To hold a particular job from being scheduled
scontrol hold <jobid>

# To release a particular job to be scheduled
scontrol release <jobid>

# To requeue (cancel and rerun) a particular job
scontrol requeue <jobid>
```


### Interactive jobs

#### srun

```sh
# aliases to make my life less miserable
alias SRUN_SIMPLE="srun --pty /bin/bash"
alias SRUN_EXPRESS="srun --ntasks=1 --cpus-per-task=4 --mem-per-cpu=4096 --partition=express --pty /bin/bash"
alias SRUN_MEDIUM="srun --ntasks=1 --cpus-per-task=4 --mem-per-cpu=4096 --partition=medium --pty /bin/bash"

SRUN_CUSTOM() {
    if [ "$#" -ne 3 ]; then
        echo "Usage:"
        echo "SRUN_CUSTOM  cpus-per-task  mem-per-cpu  partition"
    else
        srun --ntasks=1 --cpus-per-task="$1" --mem-per-cpu="$2" --partition="$3" --pty /bin/bash
    fi
}
```
