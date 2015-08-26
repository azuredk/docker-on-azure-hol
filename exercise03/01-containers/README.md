#Containers

From [Docker](https://www.docker.com/whatisdocker) we have the following definition of a Container:

> Containers include the application and all of its dependencies, but share the kernel with other containers. They run as an isolated process in userspace on the host operating system. They’re also not tied to any specific infrastructure – Docker containers run on any computer, on any infrastructure and in any cloud.

Containers are launched from Images, and as such we can think of them as running instances of an image.
Another way of looking at it, is that images are our build time constructs and containers are our runtime constructs.

We start images using the `Docker run` command, but lets look at listing and getting images first.

## Listing Containers

```
# Lists only running containers
$ docker ps 

# Lists all containers
$ docker ps -a 
```
If you followed the previous exercise you probably already have an Ubuntu image on your system. 
If not, then you can pull down a pre-built ubuntu image with the following command
```
$ docker pull ubuntu
``` 

## Running an interactive shell

Using that Ubuntu image we can run an interactive shell using the following command
```
$ docker run -i -t ubuntu /bin/bash
```
The `-i` flag starts an interactive container meaning that the prompt we enter will be from within the container.
The `-t` flag creates a pseudo-TTY that attaches `stdin` and `stdout`.

To detach the TTY without exiting the shell, use the escape sequence `Ctrl-p` + `Ctrl-q`. 
The container will continue to exist in a stopped state once exited. 
To list all containers, stopped and running use the `docker ps -a` command.

## Controlling a container

In the example below we'll assign a variable to work against a specific container

```
# Start a new container
$ JOB=$(docker run -d ubuntu /bin/sh -c "while true; do echo Hello world; sleep 1; done")

# Stop the container
$ docker stop $JOB

# Start the container
$ docker start $JOB

# Restart the container
$ docker restart $JOB

# SIGKILL a container
$ docker kill $JOB

# Remove a container
$ docker stop $JOB # Container must be stopped to remove it
$ docker rm $JOB
```

Instead of using a variable we can also use the name or short id of the container. 
If we don't specify a name then Docker will generate a random one for us. 
Using the `docker ps -a` command again, we get a list that contains both the short id and container name.

Let's create a long running container and assign it a name

```
# Start a long running process
$ docker run -d -name=my-ubuntu ubuntu /bin/sh -c "while true; do echo Hello world; sleep 1; done"

# Collect the output of the job so far
$ docker logs my-ubuntu

# Kill the job
$ docker kill my-ubuntu
```

## Inspecting our container

Lastly, we can take a low-level dive into our Docker container using the docker inspect command. 
It returns a JSON hash of useful configuration and status information about Docker containers.

```
$ docker inspect container_name
```