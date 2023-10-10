# Snakemake

## run rule locally

```py
localrules: foo
```

Can be called multiple times in Snakefile.

## Using lambda functions inside rules

Listed below are some of my examples that used lambda for conditional situations.

1. Uses lambda to define input file needed depending on wildcard 'data_source'

```py
rule extract_by_chr:
    input:
        lambda wildcards: str(EXAC_RAW_PATH / "ExAC.r1.sites.vep.vcf.gz") \
            if (wildcards.data_source.lower()=='exac') \
            else \
            str(GNOMAD_RAW_PATH / "exomes"/ "gnomad.exomes.r2.0.1.sites.vcf.gz")
    output:
        "{path}/split_by_chr/{data_source}_raw_chr_{chr_no}.vcf.gz"
    shell:
        "..."
```

2. Uses lambda to choose the parameter flag required

```py
rule extract_Adjusted_AC_AN_exac:
    output:
        str("extract_AC_AN/{filter_used}" / "11_table.tsv.gz")
    params:
        filter_parameter = lambda wildcards: '--remove-filtered-all' if wildcards.filter_used=='pass'  else ''
    shell:
        "..."
```

## Using dictionary with wildcards as keys

```py
input:
    bams = lambda wildcards: dict_name[wildcards.sample]
```

## `zip` with >2 wildcards

Use multiple `expand`. Example:

```py
expand(expand(
    ANALYSIS + "/{sample_g}_vs_{sample_t}/Stelka/results/variants/somatic.{{typevar}}_Filtered",
            zip, sample_g=GERMLINE, sample_t=TUMOR),
            typevar=TYPEVAR)
```

[Source](https://stackoverflow.com/a/48864284/3998252)

## Using bash script that sends its own job in a snakemake rule

Let's say `script.sh` sends its own job to a cluster. To use this script in a snakemake rule, use `-K` flag with `bsub`
in that shell script and then use `wait` command in snakemake rule.

```py
rule xxx:
    shell:
        """
        ./script.sh  &
        wait
        """
```

## Sourcing `.bashrc` as part of shell command in snakemake rule

```sh
set +u; source /path/to/.bashrc; set -u
```

[Source](https://stackoverflow.com/a/49681210/3998252).

## `--dag` and custom print messages

If custom print messages are used in snakemake pipeline, [`--dag`
visualization](https://snakemake.readthedocs.io/en/stable/executing/cluster-cloud.html#visualization) will run into
error.  To make them play nice, use dot's commenting in print messages.

According to [dot manual](https://www.systutorials.com/docs/linux/man/1-dot/#lbAF),
>  Comments may be /*C-like*/ or //C++-like.

Example:

```py
print (f'// yo yo yo: "{x}"')
```

## SLURM related

### Snakemake hangs when job times out or cancelled

This is a problem as [reported here](https://stackoverflow.com/q/52500725/3998252). I'm copying my answer from that site
to solve this issue:

Snakemake doesn't recognize all kinds of job statuses in slurm (and also in other job schedulers). To bridge this gap,
snakemake provides option `--cluster-status`, where custom python script can be provided. As per [snakemake's
documentation](https://snakemake.readthedocs.io/en/stable/executing/cli.html#CLUSTER):

```sh
 --cluster-status

Status command for cluster execution. This is only considered in combination with the –cluster flag.
If provided, Snakemake will use the status command to determine if a job has finished successfully or failed.
For this it is necessary that the submit command provided to –cluster returns the cluster job id.
Then, the status command will be invoked with the job id.
Snakemake expects it to return ‘success’ if the job was successfull, ‘failed’ if the job failed and ‘running’ if the job still runs.

```

[Example shown](https://snakemake.readthedocs.io/en/stable/tutorial/additional_features.html#using-cluster-status) in
snakemake's doc to use this feature:

```py
#!/usr/bin/env python
import subprocess
import sys

jobid = sys.argv[1]

output = str(subprocess.check_output("sacct -j %s --format State --noheader | head -1 | awk '{print $1}'" % jobid, shell=True).strip())

running_status=["PENDING", "CONFIGURING", "COMPLETING", "RUNNING", "SUSPENDED"]
if "COMPLETED" in output:
  print("success")
elif any(r in output for r in running_status):
  print("running")
else:
  print("failed")
```

To use this script call snakemake similar to below, where status.py is the script above.

```sh
$ snakemake all --cluster "sbatch --cpus-per-task=1 --parsable" --cluster-status ./status.py
```

---
Alternatively, you may use premade custom scripts for several job schedulers (slurm, lsf, etc), available via
[Snakemake-Profiles](https://github.com/Snakemake-Profiles/doc). Here is the one for slurm -
[slurm-status.py](https://github.com/Snakemake-Profiles/slurm/blob/master/%7B%7Bcookiecutter.profile_name%7D%7D/slurm-status.py).

### Using snakemake profile

Snakemake profiles make it easy to always use certain flags and options. Various premade
[Snakemake-Profiles](https://github.com/Snakemake-Profiles/doc) have been made available by the community/authors. For
slurm, I use my own forked repo - https://github.com/ManavalanG/slurm.

!!! note When setting up, for `submit_script`, choose `slurm-submit-advanced.py` as this [allows the usage of
    `--cluster-config` option](https://github.com/Snakemake-Profiles/slurm/issues/23#issuecomment-527379117).

### Job logs in append mode

Use `--open-mode=append` with `sbatch`.

From `sbatch` [doc](https://slurm.schedmd.com/sbatch.html):

```sh
--open-mode=append|truncate

        Open the output and error files using append or truncate mode as specified. The default value is specified by the system configuration parameter JobFileAppend.
```
