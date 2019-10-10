# Conda

- [Conda](#conda)
  - [Some basic commands](#some-basic-commands)
  - [Example environment file](#example-environment-file)

## Some basic commands

```sh
# install conda env from file
conda env create --file environment.yml

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