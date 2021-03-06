nugrid-wendocker
================

A number of Docker containers for exploring the stellar evolution, explosion and nucleosynthesis datasets that are produced by the NuGrid collaboration.

## wendocker1
The container mounts the NuGrid VOSpace for remote data access and hosts an iPython notebook server. This can be built and run as a standalone on your local machine, giving access to the notebooks via localhost:8080 (or < the ip of your VM >:8080).

This project is based on work originally performed by the NuGrid Collaboration:
[nugridstars.org](www.nugridstars.org "The NuGrid Collaboration")
and [this](https://index.docker.io/u/unfairbanks/docker-ipython-notebook) project.

## vos-readonly
The container mounts the NuGrid VOSpace and exports the volume for binding with other containers.

1. Build the data docker:

    docker build -t nugrid-vos:0.1 vos-readonly

2. Run the data docker:

    docker run -d --privileged=true --name nugrid-vos -t nugrid-vos:0.1
    
**Important**: The binding step does not work as of docker-1.2, because FUSE mounted volumes can not be shared (userspace).

## public-notebook
The container hosts an iPython notebook server with the VOSpace located at 
    
    nugrid_path = /home/nugrid/CADC/NuGrid.
    
The container requires the VOSpace volume to be connected at runtime, either from the vos-readonly container like so:

1. Build the app docker:

    docker build -t nugrid-notebook:0.1 public-notebook

2. Run the app docker:

    docker run --volume-from nugrid-vos /home/nugrid/CADC/NuGrid -p 80:8080 --name nugrid-notebook nugrid-notebook:0.1

**Important**: The binding step does not work as of docker-1.2, because FUSE mounted volumes can not be shared (userspace).

The current alternative is the following recipe, assuming the Docker host (In our case a persistent VM) has the python vos module installed:

1. Mount Nugrid VOSpace on the host:

   mkdir /vos\_nugrid
   
   vos-readonly/mount-nugrid-vos.bash /vos\_nugrid

2. Run the app docker:

   docker run --volume /vos\_nugrid:/home/nugrid/CADC/NuGrid -p 80:8080 --name nugrid-notebook nugrid-notebook:0.1

## Host-tools

This directory contains the files and information needed to set up a host machine to
manage the docker containers for both public and CANFAR-authenticated users. The host
machine creates, starts, stops and removes containers automatically, connecting
users to their personal container/notebook session on request.


