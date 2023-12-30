## Learning outcomes

**After having completed this chapter you will be able to:**

- fetch and run Docker containers on their computer.
- interpret the instructions of a Dockerfile
- create simple Docker containers to run simple python/R scripts.

## Material

TODO: add overview of necessary files, video, etc

[:fontawesome-solid-file-pdf: Download the presentation](../assets/pdf/docker_dance.pdf){: .md-button }

* Unix command line [E-utilities documentation](https://www.ncbi.nlm.nih.gov/books/NBK179288/)


## 2.1 Docker Dance

We will use Docker as an example to illustrate the development and use of containers.

### Install Docker

Please follow the installation of the latest version of Docker Desktop for your operating system. It is described at [Get Docker](https://docs.docker.com/get-docker/)

!!! Info
    Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a paid subscription.
    Note that 'commercial use' is interpreted quite broad. 

### Introducing the Dockerfile

The Dockerfile is the starting point of the Docker Dance which is schematically drawn here.

![Docker Dance](./../assets/images/6090-g1689.png){: style="width:650px;"}

Now, let's focus on the instructions for building Docker container images which are saved in a text file, named by default **Dockerfile**.

This is a basic recipe with three statements, one FROM and two RUN statements. 

To follow along on your own, copy the content of the shown `Dockerfile` into a file named `Dockerfile` and save the file on your disk.

```sh title="Dockerfile"
FROM ubuntu:18.04

RUN apt update && apt -y upgrade
RUN apt install -y wget
```

The **FROM** statement describes the parent image. Typically, an 'operating' system but you can also use an image of other parties as a starting point. This instruction creates the base layer.

```sh
FROM ubuntu:18.04
```

!!! info "Recommendation" 
    Pin the version of the OS of the base layer.
    There is an [interesting publication](https://doi.org/10.1371/journal.pcbi.1008316) regarding recommendation when manually crafting Dockerfiles. Rule 5 points out the importance of pinning versions of the base image but also system libraries or other installed software. 

The **RUN** statement specifies the command to execute inside the image filesystem.

Think about it this way: every RUN line is essentially what you would run to install programs on a freshly installed Ubuntu OS. This command will be executed as root in the container.

```sh
RUN apt install wget
```

Each row in the recipe corresponds to a **layer** of the final image.

TODO: create illustration like e.g. https://docs.docker.com/build/guide/images/layers.png
			
### Anatomy of the commands

With this basic Dockerfile, we will already start the build process which creates an image. For reference, have a look at the sketch of the Docker Dance above.

**Building Docker image**
		 								
The build command implicitly looks for a file named `Dockerfile` in the current directory:

```sh
docker build .

# or by specifying the exact file name

docker build --file Dockerfile .
```

**Syntax**: -file / -f

. stands for the context (in this case, current directory) of the build process. This makes sense if during the build process, we will copy files from local filesystem, for instance. 

!!! info
    Avoid contexts (directories) overpopulated with files (even if not actually used in the recipe).

You can define a specific name for the image during the build process.

**Syntax**: -t imagename:tag. If not defined :tag default is latest.

```sh
docker build -t mytestimage:v1 .
```

Once the build process is finished, the last line of output should be `Successfully built ... `. Then you are good to go.

As next step, we will check with the command `docker images` that you see the newly built image in the list of images.

TODO: add output of the command as screenshot

```sh
docker images
```
Then let’s check the ID of the image and run it later. But right now, we investigate some additional statements for the recipes!

Additional statements for the Dockerfile	

TODO: refine table with following content:

| command | what does it do?                 | Example                                               |
|---------|----------------------------------|-------------------------------------------------------|
| LABEL   | Who is maintaining the container image | LABEL  maintainer=”your name <your.email@domain.org>” |
| WORKDIR        | all subsequent actions will be executed in that working directory. | WORKDIR ~ |
| COPY    | lets you copy a local file or directory from your host (the machine from which you are building the image) | COPY ~/.bashrc . # COPY source destination  |
| ADD     | same, but ADD works also for URLs, and for .tar archives that will be automatically extracted upon being copied. |  |
| ARG     | available only while the image is built  | |
| ENV     | available for the future running containers | |
| ENTRYPOINT  | The ENTRYPOINT specifies a command that will always be executed when the container starts.  | |
| CMD     | The CMD specifies arguments that will be fed to the ENTRYPOINT. | |

**Further readings**

Difference between ADD and COPY explained [here](https://stackoverflow.com/questions/24958140/what-is-the-difference-between-the-copy-and-add-commands-in-a-dockerfile) and [here](https://nickjanetakis.com/blog/docker-tip-2-the-difference-between-copy-and-add-in-a-dockerile).

Difference between ARG and ENV explained [here](https://vsupalov.com/docker-arg-vs-env/).

### A more complex recipe

A more complex recipe (save it in a text file named Dockerfile:

TODO: check this part once R/Python scripts are available

```sh title="Dockerfile"
FROM ubuntu:18.04

LABEL 
WORKDIR ~

RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y wget

ENTRYPOINT ["/usr/bin/wget"]
CMD ["https://cdn.wp.nginx.com/wp-content/uploads/2016/07/docker-swarm-hero2.png"]
```

**Tips for Docker files**
						
You should try to separate the Dockerfile into as many stages as possible, because this will allow for better caching.

For example for `apt-get`:

You must run apt-get update and apt-get install in the same command, otherwise you will encounter caching issues.
Remember to use apt-get install -y, because you will have no control over the process while it’s building.


**Useful resources**

[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
[Best practices](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1008316)
[Ten simple rules for writing Dockerfiles for reproducible data science](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1008316)

TODO: add exercises difficult examples from BioInformatics ???? see Biocontainers community

### Running our Docker container

Now we want to use what is inside the image.

`docker run` creates a fresh container (active instance of the image) from a Docker (static) image, and runs it.

The format is:

```sh
docker run [docker options] <IMAGE NAME> [image arguments]
```

This means that arguments that affect the way Docker runs must always go before the image name, but arguments that are passed to the image itself must go after the image name.

```sh
docker run ubuntu:18.04 /bin/ls
```

TODO: add command which is from the built container we use above 

TODO: add command which is from the example we use in the current R/Python scripts 

**Exercise:** What happens if you execute ls in your current working directory: is the result the same?

!!! info 
    You can execute any program/command that is stored inside the image.

    ```sh
    docker run ubuntu:18.04 /bin/whoami
    docker run ubuntu:18.04 cat /etc/issue
    ```

??? done "Answer"
    Anything surprising happened?	
								
**List running containers**

```sh
docker ps
```

List all containers (whether they are running or not):

```sh
docker ps -a
```

The IDs that are shown can be useful for other docker commands like `docker stop` and `docker exec`.

### Volumes

Docker containers are fully isolated. It is necessary to mount volumes in order to handle input/output files.
By default, Docker containers cannot access data on the host system. This means you cannot use host data in your containers. All data stored in the container will be lost when the container exits

TODO: check about mount bind statements

You can solve this in two ways:

-v /path/in/host:/path/in/container: This bind mounts a host file or directory into the container. Writes to one will affect the other. Note that both paths have to be absolute paths, so you often want to use`pwd`/some/path

-v volume_name:/path/in/container. This mounts a named volume into the container, which will live separately from the rest of your files. This is preferred, unless you need to access or edit the files from the host.

```sh
mkdir datatest
touch datatest/test
docker run --detach --volume $(pwd)/datatest:/scratch --name fastqc_container biocontainers/fastqc:v0.11.9_cv7 tail -f /dev/null
docker exec -ti fastqc_container /bin/bash
> ls -l /scratch
> exit
```

TODO: Insert example exercises

## 2.2 Container registries (e.g. Docker Hub)

Images can be stored locally or shared in a registry. Docker hub is the main public registry for Docker images.
Let’s search the keyword “ubuntu”

TODO: insert screenshot of the output

There are a lot of alternatives to Docker hub for image registries depending on the needs of the organisation or company. Some examples are shown below:

TODO: insert image of the registries


1. Get the latest image or latest release

```sh
docker pull ubuntu
```

TODO: add output

2. Check the versions of Ubuntu present and fetch version 18.04 using tags

TODO: add screenshot

```sh
docker pull ubuntu:18.04
```

When you ran this command, Docker first looked for the image on your local machine, and when it couldn’t find it, pulled it down from a cloud registry of Docker images called Docker Hub
		 			
What other repositories are possible?
Have a look at the web site https://biocontainers.pro/ which is a specific directory of Bioinformatics related tools.
the images are stored in Docker hub and/or Quay.io (RedHat registry)
these images are normally created from [Bioconda](https://bioconda.github.io)

Example: FastQC
https://biocontainers.pro/#/tools/fastqc

TODO: Open solution
```sh
docker pull biocontainers/fastqc:v0.11.9_cv7
```

Images can be listed by the command

```sh
docker images
docker image ls
```
Each image has a unique IMAGE ID.

TODO: add image with example

Where are these images stored? On Linux, they usually go to /var/lib/.
Docker is very greedy in storage so regular cleaning is necessary. We will see later on how you can do the purging.
Sometimes, it is also useful to get more information about the images. You can do this via

```sh
docker image inspect
```
