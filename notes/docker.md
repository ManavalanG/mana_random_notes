- [docker](#docker)
    - [Basic (sorta) pipeline](#basic-sorta-pipeline)
    - [Some `docker run` options/args](#some-docker-run-optionsargs)


# docker


## Basic (sorta) pipeline

```
# build docker in workdir
docker build -f Dockerfile -t image_name .

# save to allow reloading conveniently as needed
docker docker save image_name > image_name.tar

# load docker
docker load --input image_name.tar
 
# run docker
docker run image_name
```


## Some `docker run` options/args

[Source](https://docs.docker.com/engine/reference/commandline/run/)

```
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