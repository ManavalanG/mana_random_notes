# Random notes

This is a place to store random notes, tips and tricks I find useful for the future me. 

Topics:

- [Python](./docs/python.md)
- [Jupyter notebook](./docs/jupyter_notebook.md)
- [Snakemake](./docs/snakemake.md)
- [Bash scripting](./docs/bash.md)
- [Conda](./docs/conda.md)
- [Containers (docker, singularity)](./docs/containers.md)
- [Terminal related](./docs/terminal.md)
- [SLURM related](./docs/slurm.md)
- [Bioinformatics related](./docs/bioinformatics.md)
- [Other random stuff](./docs/random.md)


## Use mkdocs to serve

[mkdocs](https://www.mkdocs.org/) is used to serve up the static website. Commands used:

```sh
# create  conda env
conda env create --file "configs/envs/mkdocs.yaml"

# activate env
conda activate mkdocs

# install more stuff via pip (not ideal but good enough for now)
pip install -r "configs/mkdocs/requirements.txt"

# now build website using website and serve live (locally)
mkdocs serve
```