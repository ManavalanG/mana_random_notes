# Conda

## Some basic commands

```sh
# install conda env from file
conda env create --file environment.yml

# update existing conda env. --prune uninstalls packages that were removed.
# Doesn't work on pip installed stuff though.
conda env update --file environment.yml

# activate env
conda activate env_name

# deactivate env
conda deactivate env_name

# currently active conda environments
conda env list

# remove conda environment
conda env remove -n env_name
```

## Example environment file

```yaml
name: playenv

channels:
- conda-forge

dependencies:
- python=3.6
- pip
- pip:
    - colorlog
```

## ready and go envs

Meant to use for random, one-off and minor task scenarios that don't justify work spent on creating project/task/repo
specifc conda envs.

### Python stuff

```yaml
name: generic_python_env

channels:
  - conda-forge

dependencies:
  - python==3.7.9
  - pandas==1.3.4
  - numpy==1.21.4
  - scipy==1.7.3
  - matplotlib==3.5.0
  - seaborn==0.11.2
  - openpyxl==3.0.9
  - xlrd==2.0.1
  - pyyaml==6.0
  - black==21.11b0
  - pylint==2.12.1
  - pytest==6.2.5
  - pytest-xdist==2.4.0
  - pytest-cov==3.0.0
  - fire==0.4.0
  - typer==0.4.0
  - colorlog==6.6.0
```

### Bio stuff

```yaml
name: biotools

channels:
  - conda-forge
  - bioconda

dependencies:
  - samtools==1.13
  - htslib==1.13
  - bcftools==1.13
  - bedtools=2.30.0
  - seqtk==1.3
  - pysam==0.17.0
```
