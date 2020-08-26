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
alias SQ='squeue -o "%.8i %.20j %.10P %.7u %.5D %.11M  %.11l %.3t %.11m %R" -u $USER'
alias SQ_long='squeue -o "%.8i %.20j %.10P %.7u %.5D %.11M  %.11l %.3t %.11m %R %V %o" -u $USER'  #also shows submission time and command ran
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

# sacct aliases I find useful
alias SC='sacct --format="JobID,JobName,Ntasks,MaxRSS,Elapsed,state,NodeList,ReqMem,MaxVMSize,AveVMSize,Partition,AllocTRES%40" --units=M'
```


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
