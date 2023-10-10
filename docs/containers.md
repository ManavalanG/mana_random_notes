# Containers

## docker

### Basic (sorta) pipeline

```sh
# build docker in workdir
docker build -f Dockerfile -t image_name .

# save to allow reloading conveniently as needed
docker docker save image_name > image_name.tar

# load docker
docker load --input image_name.tar

# run docker
docker run image_name
```

### Some `docker run` options/args

[Source](https://docs.docker.com/engine/reference/commandline/run/)

```sh
--workdir , -w 		Working directory inside the container

--volume , -v 		Bind mount a volume

# use this to remove file created in the end of docker session (learnt this the hard way)
--rm 		Automatically remove the container when it exits

# to avoid writing files as root
--user , -u 		Username or UID (format: <name|uid>[:<group|gid>])

--entrypoint 		Overwrite the default ENTRYPOINT of the image

--tty , -t 		Allocate a pseudo-TTY

--interactive , -i 		Keep STDIN open even if not attached

# use -it for interactive seesion
```

## Singularity

### Basic stuff

```sh
# download pre-built images
singularity pull  --name hello.simg  shub://vsoch/hello-world   # pull with custom name

# for docker images
singularity pull docker://godlovedc/lolcow  # with default name

```

### Mounting home dir

By default singularity bind mounts `/home/$USER`, `/tmp`, and `$PWD` into your container at runtime.
[Source](https://singularity.lbl.gov/quickstart#working-with-files).

If mounting home is undesirable, it may be turned off. For `singularity run`, these options are available:

```sh
    -H|--home <spec>    A home directory specification.  spec can either be a
                        src path or src:dest pair.  src is the source path
                        of the home directory outside the container and dest
                        overrides the home directory within the container
    --no-home           Do NOT mount users home dire
```

* Use `--no-home` if mounting home dir is not needed or undesirable.
* USe `--home` if the container expects a home directory. Example usage: ``` --home `pwd`/abcd:$HOME```

### Avoid filing up `/tmp`

Singularity by default mounts `/tmp` dir. In case of cluster, IT would be happy if temporary directory is pointed
instead to a scratch directory, as singularity containers fill `/tmp` up fast. This can be changed by:

* Set `export SINGULARITY_TMPDIR=path_to_dir`. Or, [is it
  `SINGULARITY_CACHEDIR`](https://singularity.lbl.gov/faq#no-space-left-on-device)?
* When binding/mounting directories is possible (for example `singularity run`), use ``` --home `pwd`/tmp_dir:/tmp```.
  IT seems to prefer this solution.
