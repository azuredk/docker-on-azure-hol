#Dockerfile

A Dockerfile is the template for our images. It describes how the image (or ultimately container) should be configured, which software to install and what to run upon startup.

In this section we'll mostly dive into building our own image, but its highly recommended to read "[Dockerfile Best Practices](https://docs.docker.com/articles/dockerfile_best-practices/)" from the documentation pages.

##Building a Dockerfile

So lets start off by building a simple image. We'll do this by creating a `Dockerfile` that contains
a set of instructions that tells Docker how to build our image.

First we need to create a directory with a `Dockerfile` in it.

```
$ mkdir sinatra
$ cd sinatra
$ touch Dockerfile
```

We'll use the example from the Docker documentation pages on how to build a Sinatra image.

```
FROM ubuntu:14.04
MAINTAINER John Doe <john-doe@example.com>
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
``` 

Each instruction within the Dockerfile prefixes a statement and is capitalized.

The first instruction `FROM` tells Docker what we want to base our image on, and in this case we based our image on an existing Ubuntu 14.04 image.

The `MAINTAINER` part is just meta data, which tells others (reading the Dockerfile) who maintains this image.

Finally we have two `RUN` instructions. Each of these executes a command inside the image, which is commonly used to install packages.
In the example above we are updating the APT cache, installing Ruby and RubyGems, and then the next `RUN` instruction installs sinatra using `gem install`.

> There are a lot of instructions available to use in a Dockerfile, so its a good idea to checkout the [reference documentation for Dockerfiles](https://docs.docker.com/reference/builder).

Now that we have a `Dockerfile` we can build it using the `docker build` command. And this is done from within the sinatra folder we created.

```
$ docker build -t ouruser/sinatra:v2 .
```

In the command above we have specified the `-t` flag to identity our image as belonging to the user `ouruser`, and the repository name `sinatra`
and gave it the tag `v2`. We’ve also specified the location of our Dockerfile using the `.` to indicate a Dockerfile in the current directory.

>**Note:** An image can’t have more than 127 layers regardless of the storage driver. This limitation is set globally to encourage optimization of the overall size of images.

When the build process is done you can run the newly created image with the following command

```
$ docker run -t -i ouruser/sinatra:v2 /bin/bash
``` 

##Dockerfile for ASP.NET 5 beta 6

A good way of learning more about how a Dockerfile can be used to build your own image, is to browse the [Docker Hub](https://hub.docker.com) and inspect the Dockerfile that is available for all public repos.

Lets look at the Dockerfile for [aspnet](https://hub.docker.com/r/microsoft/aspnet/), which is available on [github](https://github.com/aspnet/aspnet-docker/blob/master/1.0.0-beta6/Dockerfile) (this one is for beta6).

```
FROM mono:4.0.1

ENV DNX_VERSION 1.0.0-beta6
ENV DNX_USER_HOME /opt/dnx

RUN apt-get -qq update && apt-get -qqy install unzip

RUN curl -sSL https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.sh | DNX_USER_HOME=$DNX_USER_HOME DNX_BRANCH=v$DNX_VERSION sh
RUN bash -c "source $DNX_USER_HOME/dnvm/dnvm.sh \
	&& dnvm install $DNX_VERSION -a default \
	&& dnvm alias default | xargs -i ln -s $DNX_USER_HOME/runtimes/{} $DNX_USER_HOME/runtimes/default"

# Install libuv for Kestrel from source code (binary is not in wheezy and one in jessie is still too old)
RUN apt-get -qqy install \
	autoconf \
	automake \
	build-essential \
	libtool
RUN LIBUV_VERSION=1.4.2 \
	&& curl -sSL https://github.com/libuv/libuv/archive/v${LIBUV_VERSION}.tar.gz | tar zxfv - -C /usr/local/src \
	&& cd /usr/local/src/libuv-$LIBUV_VERSION \
	&& sh autogen.sh && ./configure && make && make install \
	&& rm -rf /usr/local/src/libuv-$LIBUV_VERSION \
	&& ldconfig

ENV PATH $PATH:$DNX_USER_HOME/runtimes/default/bin
```

As you can see its built `FROM` the mono 4.0.1 image. Then it sets two environment varibles using the `ENV` instruction.
And as we've seen before it uses `RUN` multiple times. In this case it starts off by updating the APT cache, installing the unzip package, 
then downloads a shell script for installing the DNVM (DotNet Version Manager), and then runs a bash command.

In the two next `RUN` instructions it gets and installs libuv, which is used for Kestrel. Lastly, it sets another environment variable.

In one of the later exercises we'll look at creating a new image for an ASP.NET 5 site, which is based on this `aspnet` image, in order to run an ASP.NET 5 site in a Docker container. 