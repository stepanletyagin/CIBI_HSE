# 1 - Dependencies management

***git branch name:*** dependencies

## Theory [2]

As usual, we will start with a few theoretical questions:

* [0.5] What is Docker, and how it differs from dependencies management systems? From virtual machines?

Docker is a software platform that allows one to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime.

Docker containers are very easy to deploy in any cloud platform. It can get more applications running on the same hardware when compared to other dependencies management systems, it makes it easy for developers to quickly create, ready-to-run containerized applications and it makes managing and deploying applications much easier.

VMs (virtual machines) have the host OS (operating system) and guest OS inside each VM. A guest OS can be any OS, irrespective of the host OS. In contrast, Docker containers host on a ***single physical server*** with a host OS, which shares among them. Sharing the host OS between containers makes them light and increases the boot time.

The second difference between VMs and Docker is that ***Virtual Machines are stand-alone with their kernel*** and security features. Therefore, applications needing more privileges and security run on virtual machines. 

Docker container packages are self-contained and can run applications in any environment, and since they don’t need a guest OS, they can be ***easily ported across different platforms***. Docker containers can be easily deployed in servers since containers being lightweight can be started and stopped in very less time compared to virtual machines.

-----

* [0.5] What are the advantages and disadvantages of using containers over other approaches?

***Advantages:***
 
1.  Less overhead. Containers require less system resources than traditional or hardware virtual machine environments because they don't include operating system images.
2. More consistent operation. DevOps teams know applications in containers will run the same, regardless of where they are deployed.
3. Flexible resource sharing.
4. Greater efficiency. Containers allow applications to be more rapidly deployed, patched, or scaled. 

***Disadvantages:***

1.   Difficult to manage large amount of containers.
2.   Not right for all tasks. Some applications simply need to be monolithic - they're designed that way, and benefits like scalability and fast deployment don't readily apply.
3. Limited tools. The kind of tools needed to monitor and manage containers are still lacking in the industry.

-----

* [0.5] Explain how Docker works: what are Dockerfiles, how are containers created, and how are they run and destroyed?

A Dockerfile is a text document (without a file extension) that contains the instructions to set up an environment for a Docker container. You can build a Docker image using a Dockerfile.

Now, let's see how Docker containers are created, run and destroyed.

The comand that is used to [create](https://docs.docker.com/engine/reference/commandline/create/) container: 
```
docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
# or without starting it:
docker create
```
When creating a container, the docker daemon creates a writeable container layer over the specified image and prepares it for running the specified command.

The comand that is used to [run](https://docs.docker.com/engine/reference/commandline/run/) container: 

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
The docker run command first creates a writeable container layer over the specified image, and then starts it using the specified command.

To [destroy](https://docs.docker.com/engine/reference/commandline/kill/) one ore more containers:
```
docker kill [OPTIONS] CONTAINER [CONTAINER...]
```

See [(source)](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) for more.

-----

* [0.25] Name and describe at least one Docker competitor (i.e., a tool based on the same containerization technology).

***Podman***

Podman is an open-source, Linux-native tool designed to develop, manage, and run containers and pods under the Open Container Initiative (OCI) standards. Presented as a user-friendly container orchestrator developed by Red Hat, Podman is the default container engine in RedHat 8 and CentOS 8.

It is one of a set of command-line tools designed to handle different tasks of the containerization process, that can work as a modular framework. It includes: pods and container image manger, a container builder, a container image inspection manager, container runner and feature builder.

Podman has a different conceptual approach to containers. As hinted by the name, Podman can create container "pods" that work together, a feature resembling the Kubernetes pods. Pods organize separate containers under a common denomination to manage them as single units.

The main benefit is that developers can share resources, using different containers for the same application inside a pod: a container for the frontend, another for the backend, and a database.

Podman has unique advantages as a development and management tool that makes it a viable and interesting alternative to Docker in the appropriate context.

-----

* [0.25] What is conda? How it differs from apt, yarn, and others?

Сonda is an open source package management system and environment management system for installing multiple versions of software packages and their dependencies and switching easily between them. Conda allows users to easily install different versions of binary software packages and any required libraries appropriate for their computing platform. Also, it allows users to switch between package versions and download and install updates from a software repository.

First things first, conda is a system package manager and with conda you can install much more than just Python libraries. You can install R, R libraries, Node.js, Java programs, C and C++ programs and libraries, Perl programs, the list is pretty long and limitless. So conda isn't limited with programming language, unlike, for example *pip*.

If your goal is to develop software using the Python scientific stack, then you should consider using Conda, beacause of it's popularity in community. 

By default, conda and all packages it installs, are installed locally with a user-specific configuration. Administrative privileges are not required, and no upstream files or other users are affected by the installation, unlike apt.

Check [this](https://askubuntu.com/questions/804544/what-are-the-differences-similarities-between-multiple-python-package-management) out.

-----

## Problem [6.5]

The problem itself is relatively simple. 

Imagine that you developed an excellent RNA-seq analysis pipeline and want to share it with the world. Based on your experience, you are confident that the popularity of the pipeline will be proportional to its ease of use. So, you decided to help your future users and to pack all dependencies in a Conda environment and a Docker container.

Here is the list of tools and their versions that are used in your work:
* [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), v0.11.9
* [STAR](https://github.com/alexdobin/STAR), v2.7.10b
* [samtools](https://github.com/samtools/samtools), v1.16.1
* [picard](https://github.com/broadinstitute/picard), v2.27.5
* [salmon](https://github.com/COMBINE-lab/salmon), commit tag 1.9.0
* [bedtools](https://github.com/arq5x/bedtools2), v2.30.0
* [multiqc](https://github.com/ewels/MultiQC), v1.13


**Anaconda**:

* [1] Install conda, create a new virtual environment, and install all necessary packages. 
* [0.75] You won't be able to install some tools - that's fine. List their names, and explain what should be done to make them conda-friendly ([conda-forge](https://conda-forge.org/docs/maintainer/adding_pkgs.html) channel, [bioconda](https://bioconda.github.io/contributor/workflow.html) channel). 
* [0.25] [Export](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#exporting-the-environment-yml-file) the environment ([example](https://github.com/nf-core/clipseq/blob/master/environment.yml)) to the file and verify that it can be [rebuilt](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file) from the file without problems.


**Docker**:
* [3] Create a Dockerfile for a container with **all** required dependencies. Don't forget about comments; test that all tools are accessible and work inside the container. Hints:
 - If needed, grant rights to execute downloaded/compiled binaries using chmod (`chmod a+x BINARY_NAME`)
 - Move all executables to $PATH folders (e.g.`/usr/local/bin`) to make them accessible without specifying the full path.
 - Typical command to run a container interactively (`-it`) and delete on exit(`--rm`): `docker run --rm -it name:tag`
* [1] Use [hadolint](https://hadolint.github.io/hadolint/) and remove as many reported warnings as possible.
* [0.5] Add relevant [labels](https://docs.docker.com/engine/reference/builder/#label), e.g. maintainer, version, etc. ([hint](https://medium.com/@chamilad/lets-make-your-docker-image-better-than-90-of-existing-ones-8b1e5de950d))

-----

### Install conda

To install Conda on my OS I used instruction [here](https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html).

For testin my installation I did comand in my terminal:

```
conda list
```

to see the list of installed packages.

-----

### Create a new virtual environment

To do that step, I performed: 

```
conda create -n CIBP_assgmnt_1 --no-default-packages
Proceed ([y]/n)? y
```
environment location:
Users/stepanletyagin/opt/anaconda3/envs/CIBP_assgmnt_1

```
conda activate CIBP_assgmnt_1 # to activate environment 
```

-----

### Install all necessary packages

Actually, I wasn't able to get any of these packages at all at first, not just some of them :)

As soon as 'defaults' already in 'channels' list I performed:

```
conda config --add channels bioconda
conda config --add channels conda-forge
```

if 'defaults' are not in 'channels', do this before :

```
conda config --add channels defaults
```
[source](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/channels.html)

and then I did:

```
conda install -y fastqc=0.11.9
conda install -y star=2.7.10b
conda install -y samtools=1.16.1 
conda install -y picard=2.27.5
conda install -y salmon=1.9.0
conda install -y bedtools=2.30.0
conda install -y multiqc=1.13
```

And it worked completely fine, but too for too much time. 

-----

### Export the environment

To do that, I performed: 

```
conda env export --from-history > env_out.yml
```

To deactivate and remove environment name:

```
conda deactivate
conda remove --name CIBP_assgmnt_1 --all
```

To rebuild environment from file:

```
conda env create -f env_out.yml
```

### env_out.yml

```
name: CIBP_assgmnt_1
channels:
  - bioconda
  - conda-forge
  - defaults
dependencies:
  - fastqc=0.11.9
  - star=2.7.10b
  - samtools=1.16.1
  - picard=2.27.5
  - salmon=1.9.0
  - bedtools=2.30.0
  - multiqc=1.13
prefix: /Users/stepanletyagin/opt/anaconda3/envs/CIBP_assgmnt_1
```
-----

### Docker

I've already installed docker a couple of years ago, but I did something like [this](https://docs.docker.com/desktop/install/mac-install/) for having installed it on macOS.

And for the Docker file:

```
touch Dockerfile 
vim Dockerfile # to edit
```
to run my Dockerfile I used:

```
docker build -t cibp_assgmnt_1 .
```
where -t cibp_assgmnt_1 specifies the image name and . symbol means that the Dockerfile already in current location.

<!-- I'll note right away, I understand that ubuntu does not need wget, but my terminal gave an error something like "wget: not found", and when I did add it manually, so everything worked. -->
Here's what I did in Dockerfile:
```
################## BASE IMAGE ######################
FROM ubuntu

################## METADATA ######################
LABEL maintainer=<stepanletyagin@gmail.com>

################## MAINTAINER ######################
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update &&\
apt-get install -y apt-utils &&\
apt-get install -y apt-transport-https &&\
apt-get install -y openjdk-11-jre-headless &&\
apt-get install -y unzip &&\ 
apt-get install -y python3-pip

RUN touch /.bashrc

ARG FASTQCVER=0.11.9

RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v${FASTQCVER}.zip && \
    unzip fastqc_v${FASTQCVER}.zip && rm fastqc_v${FASTQCVER}.zip && \
    chmod a+x FastQC/fastqc && \
    echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

ARG STARVER=2.7.10b

RUN wget https://github.com/alexdobin/STAR/releases/download/${STARVER}/STAR_${STARVER}.zip && \
    unzip STAR_${STARVER}.zip && rm STAR_${STARVER}.zip && \
    chmod a+x STAR_${STARVER}/Linux_x86_64_static/STAR && \
    mv STAR_${STARVER}/Linux_x86_64_static/STAR /bin/STAR && rm -r STAR_${STARVER}

ARG SAMTOOLSVER=1.16.1

RUN wget https://github.com/samtools/samtools/archive/refs/tags/${SAMTOOLSVER}.zip -O ./samtools-1.16.1.zip && \
    unzip samtools-${SAMTOOLSVER}.zip && rm samtools-${SAMTOOLSVER}.zip && \
    mv samtools-${SAMTOOLSVER}/misc samtools && rm -r samtools-${SAMTOOLSVER} && \
    echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

ARG PICARDVER=2.27.5

RUN wget https://github.com/broadinstitute/picard/releases/download/${PICARDVER}/picard.jar -O /bin/picard.jar && \
    chmod a+x /bin/picard.jar && \
    echo 'alias picard="java -jar /bin/picard.jar"' >> /.bashrc

ARG SALMONVER=1.9.0

RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v${SALMONVER}/salmon-${SALMONVER}_linux_x86_64.tar.gz && \
    tar -zxvf salmon-${SALMONVER}_linux_x86_64.tar.gz && rm salmon-${SALMONVER}_linux_x86_64.tar.gz && \
    chmod a+x salmon-${SALMONVER}_linux_x86_64/bin/salmon && mv salmon-${SALMONVER}_linux_x86_64/bin/salmon /bin/salmon && \
    rm -r salmon-${SALMONVER}_linux_x86_64 

ARG BEDTOOLSVER=2.30.0

RUN wget https://github.com/arq5x/bedtools2/releases/download/v${BEDTOOLSVER}/bedtools.static.binary -O /bin/bedtools.static.binary && \
    chmod a+x /bin/bedtools.static.binary && \
    echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

RUN pip install multiqc==1.13

```
To delete after exit:
```
docker run --rm -it cibp_assgmnt_1
```

-----

Then, here's my docker file after I checked it with [linter](https://hadolint.github.io/hadolint/) and made some improvements ([source](http://label-schema.org/rc1/) for specifying metadata):
```
################## BASE IMAGE ######################
FROM ubuntu:20.04

################## METADATA ######################
LABEL maintainer=<stepanletyagin@gmail.com>
LABEL org.label-schema.schema-version="1.0"
LABEL org.label-schema.build-date="2022-12-18T05:36:23Z"
LABEL org.label-schema.description="CIBI HSE HW1"

################## MAINTAINER ######################
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update &&\
apt-get install -y --no-install-recommends apt-utils=2.0.2ubuntu0.2 &&\
apt-get install -y --no-install-recommends apt-transport-https=2.0.2ubuntu0.2 &&\
apt-get install -y --no-install-recommends openjdk-11-jre-headless=11.0.17+8-1ubuntu2~20.04 &&\
apt-get install -y --no-install-recommends unzip=6.0-25ubuntu1.1 &&\ 
apt-get install -y --no-install-recommends python3-pip=20.0.2-5ubuntu1.5 &&\
apt-get clean &&\
rm -rf /var/lib/apt/lists/*

RUN touch /.bashrc

ARG FASTQCVER=0.11.9

RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v${FASTQCVER}.zip && \
    unzip fastqc_v${FASTQCVER}.zip && \
    rm fastqc_v${FASTQCVER}.zip && \
    chmod a+x FastQC/fastqc && \
    echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

ARG STARVER=2.7.10b

RUN wget https://github.com/alexdobin/STAR/releases/download/${STARVER}/STAR_${STARVER}.zip && \
    unzip STAR_${STARVER}.zip && \
    rm STAR_${STARVER}.zip && \
    chmod a+x STAR_${STARVER}/Linux_x86_64_static/STAR && \
    mv STAR_${STARVER}/Linux_x86_64_static/STAR /bin/STAR && \
    rm -r STAR_${STARVER}

ARG SAMTOOLSVER=1.16.1

RUN wget https://github.com/samtools/samtools/archive/refs/tags/${SAMTOOLSVER}.zip -O ./samtools-1.16.1.zip && \
    unzip samtools-${SAMTOOLSVER}.zip && \
    rm samtools-${SAMTOOLSVER}.zip && \
    mv samtools-${SAMTOOLSVER}/misc samtools && \
    rm -r samtools-${SAMTOOLSVER} && \
    echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

ARG PICARDVER=2.27.5

RUN wget https://github.com/broadinstitute/picard/releases/download/${PICARDVER}/picard.jar -O /bin/picard.jar && \
    chmod a+x /bin/picard.jar && \
    echo 'alias picard="java -jar /bin/picard.jar"' >> /.bashrc

ARG SALMONVER=1.9.0

RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v${SALMONVER}/salmon-${SALMONVER}_linux_x86_64.tar.gz && \
    tar -zxvf salmon-${SALMONVER}_linux_x86_64.tar.gz && \
    rm salmon-${SALMONVER}_linux_x86_64.tar.gz && \
    chmod a+x salmon-${SALMONVER}_linux_x86_64/bin/salmon && \
    mv salmon-${SALMONVER}_linux_x86_64/bin/salmon /bin/salmon && \
    rm -r salmon-${SALMONVER}_linux_x86_64 

ARG BEDTOOLSVER=2.30.0

RUN wget https://github.com/arq5x/bedtools2/releases/download/v${BEDTOOLSVER}/bedtools.static.binary -O /bin/bedtools.static.binary && \
    chmod a+x /bin/bedtools.static.binary && \
    echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

RUN pip install multiqc==1.13

```

p.s. Almost all reported warnings were related to version specification. For version control-check, I used also this [source](https://packages.ubuntu.com/ru/focal/python3-pip).

-----

## Extra points [1.5]

You will be awarded extra points for the following:
* [0.5] Using [multi-stage builds](https://docs.docker.com/build/building/multi-stage/) in Docker. E.g. to build STAR and copy only the executable to the final image.
-----

* [0.75] Minimizing the size of the final Docker image. That is, removing all intermediates, unnecessary binaries/caches, etc. Don't forget to compare & report the final size before and after all the optimizations.

```
docker build --no-cache -t cibp_assgmnt_1_min .
```
The best I could get with runnig this command was:

Original Dockerfile size: ***1.44GB***

Minimized Dockerfile size: ***1.06GB***

<img width="636" alt="Screenshot 2022-12-19 at 6 17 55 PM" src="https://user-images.githubusercontent.com/82548512/208459026-67e4174d-9ec3-43a7-8a05-406165bd4c5a.png">

-----
* [0.25] Create an extra Dockerfile that starts from [a conda base image](https://hub.docker.com/r/continuumio/anaconda3) and builds everything from your conda environment file. 

Hint: `conda env create --quiet -f environment.yml && conda clean -a` ([example](https://github.com/nf-core/clipseq/blob/master/Dockerfile))
