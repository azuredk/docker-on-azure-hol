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

##Dockerfile for ASP.NET Core

A good way of learning more about how a Dockerfile can be used to build your own image, is to browse the [Docker Hub](https://hub.docker.com) and inspect the Dockerfile that is available for all public repos.

Lets look at a base Dockerfile for [aspnet](https://hub.docker.com/r/microsoft/aspnetcore/), which is available on [github](https://github.com/aspnet/aspnet-docker).

```dockerfile
# base
FROM microsoft/dotnet:1.1.0-runtime

# set env var for packages cache
ENV DOTNET_HOSTING_OPTIMIZATION_CACHE /packagescache

COPY ./build-cache.sh /packagescache/build-cache.sh

# set up package cache
RUN /packagescache/build-cache.sh https://dist.asp.net/packagecache/1.1.0/aspnetcore.packagecache-1.1.0-legacy-debian.8-x64.tar.gz \
    && rm /packagescache/build-cache.sh

# set up network
ENV ASPNETCORE_URLS http://+:80
```

As you can see its built `FROM` Microsoft .NET Core image. Then it downloads ASP.NET Core and puts it all into a `packagescache` folder.
Then it exposes an environment variable for using the `packagescache`.

Lastly it sets an environment variable for the URLs it will expose.
