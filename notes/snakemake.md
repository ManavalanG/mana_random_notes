- [run rule locally](#run-rule-locally)
- [Using lambda functions inside rules](#using-lambda-functions-inside-rules)
- [Using dictionary with wildcards as keys](#using-dictionary-with-wildcards-as-keys)
- [`zip` with >2 wildcards](#zip-with-2-wildcards)
- [Using bash script that sends its own job in a snakemake rule](#using-bash-script-that-sends-its-own-job-in-a-snakemake-rule)
- [Sourcing `.bashrc` as part of shell command in snakemake rule](#sourcing-bashrc-as-part-of-shell-command-in-snakemake-rule)


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

Let's say `script.sh` sends its own job to a cluster. To use this script in a snakemake rule, use `-K` flag with `bsub` in that shell script and then use `wait` command in snakemake rule.

```py
rule xxx:
    shell:
        """
        ./script.sh  &
        wait
        """
```


## Sourcing `.bashrc` as part of shell command in snakemake rule

```set +u; source /path/to/.bashrc; set -u```

[Source](https://stackoverflow.com/a/49681210/3998252)


