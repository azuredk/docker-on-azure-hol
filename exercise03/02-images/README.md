#Images

In the previous section we saw that Images are the basis of containers. 
And from [Docker](https://docs.docker.com/introduction/understanding-docker/) we have the following definition of Images:

>A Docker image is a read-only template. For example, an image could contain an Ubuntu operating system with Apache and your web application installed. Images are used to create Docker containers. Docker provides a simple way to build new images or update existing images, or you can download Docker images that other people have already created. Docker images are the build component of Docker.

So far we have only used pre-built Images like the Ubuntu image, but in this section we'll dive a bit more into working with images.

##Listing images on the host
Let’s start with listing the images we have locally on our host. You can do this using the `docker images` command like so:

```
$ docker images
REPOSITORY       TAG      IMAGE ID      CREATED      VIRTUAL SIZE
training/webapp  latest   fc77f57ad303  3 weeks ago  280.5 MB
ubuntu           13.10    5e019ab7bf6d  4 weeks ago  180 MB
ubuntu           saucy    5e019ab7bf6d  4 weeks ago  180 MB
ubuntu           12.04    74fe38d11401  4 weeks ago  209.6 MB
ubuntu           precise  74fe38d11401  4 weeks ago  209.6 MB
ubuntu           12.10    a7cf8ae4e998  4 weeks ago  171.3 MB
ubuntu           quantal  a7cf8ae4e998  4 weeks ago  171.3 MB
ubuntu           14.04    99ec81b80c55  4 weeks ago  266 MB
ubuntu           latest   99ec81b80c55  4 weeks ago  266 MB
ubuntu           trusty   99ec81b80c55  4 weeks ago  266 MB
ubuntu           13.04    316b678ddf48  4 weeks ago  169.4 MB
ubuntu           raring   316b678ddf48  4 weeks ago  169.4 MB
ubuntu           10.04    3db9c44f4520  4 weeks ago  183 MB
ubuntu           lucid    3db9c44f4520  4 weeks ago  183 MB
```

The list above shows us a few details about the various images, like the *repository*, which the image came from, the *tag* for each image and the *Id* of the image.

A repository can hold many different versions of an image, and you can specify which version to use when running a container from a given image. Example:

```
# Run an ubuntu container using the 14:04 tag
$ docker run -t -i ubuntu:14.04 /bin/bash

# Run an ubuntu container using the 12:04 tag
$ docker run -t -i ubuntu:12.04 /bin/bash
```

>If you're getting an error like `cannot enable tty mode on non tty input`- just prepend the command with `winpty` yielding:
```
$ winpty docker run -i -t ubuntu:14.04 /bin/bash
$ winpty docker run -i -t ubuntu:12.04 /bin/bash
```

>Depending on the bash solution you're using, you might get an error like this: `Container command not found or does not exist`
>If this is the case, it is basically trying to resolve the path locally before sending it to the server - resulting in it not finding bash at all. 
>The easy fix to this is just to enter `bash`instead of `/bin/bash`
```
$ winpty docker run -i -t ubuntu:14.04 bash
$ winpty docker run -i -t ubuntu:12.04 bash
```


*If you don't have a given image or a given tag (version) of an image when running it with the above command it will be pulled down automatically.*

> Its recommended by Docker to always use a specific tagged image, for example `ubuntu:12.04`. That way you always know exactly what variant of an image is being used.


##Finding images

You can find images on the [Docker Hub](https://hub.docker.com/) website. Here you can browse and search official repositories for images such as [Ubuntu](https://hub.docker.com/_/ubuntu/), [NodeJs](https://hub.docker.com/_/node/) and of course [ASP.NET 5](https://hub.docker.com/r/microsoft/aspnet/) as well.

You can also search for images on the command line using the `docker search` command.

```
$ docker search aspnet 
```

##Pulling our image

If we wanted to pull down the official Microsoft ASP.NET 5 image we could use the following command:

```
$ docker pull microsoft/aspnet
```

We can now run the image on our Azure VM or locally within our virtual machine (from the private lab) like so:
```
$ docker run -t -i microsoft/aspnet /bin/bash
```

>Worst case scenario, depending on your bash - you might end up having to write:
```
$ winpty docker run -t -i microsoft/aspnet bash
```
> There is also the odd chance of getting an error from docker telling:
> `docker: Error parsing reference: "microsoft\\aspnet" is not a valid repository/tag`
> In this case we can either reference the image by its ID, which can be found using `docker images` or providing an alias by doing `docker tag microsoft/aspnet aspnet` giving it an alias of `aspnet`


##Updating an image

There are two ways of updating an image. We can either create a 
Dockerfile with the instructions needed to create the desired 
image, or we can commit our changes to an image. 
Since we'll be diving into Dockerfiles in the next section we'll 
look at commiting changes to an image here.

To update an image we first need to create a container from the 
image we'd like to update.

```
$ docker run -t -i microsoft/aspnet /bin/bash
root@0b2616b0e5a8:/#
```

>**Note:** Take note of the container ID that has been created, `0b2616b0e5a8`, as we'll need it in a moment (this is the short id of the container).

Inside our running container we can add a few files or install something using `apt-get`. 
For simplicity we'll just create a few files, and once that is done we'll exit our container using the `exit` command.

Now that we have an image with the few changes that we made. 
We can then commit a copy of this container to an image using 
the `docker commit` command.

```
$ docker commit -m "Added json files" -a "Morten Christensen" \
0b2616b0e5a8 ouruser/aspnet:v2
4f177bd27a9ff0f6dc2a830403925b5360bfe0b93d476f7fc3231110e7f71b1c
```

Here we've used the docker commit command. We've specified two flags: 
`-m` flag which allows us to specify a commit message, 
and the `-a` flag which allows us to specify an author for our update.

We've also specified the container we want to create this new image from, 
`0b2616b0e5a8` (the Id we noted earlier) and we've specified a target for the image as well:

```
ouruser/aspnet:v2
```

Now, lets list our images again using the `docker images` command. 
The list should now contain the image that was pulled and our updated version under "ouruser".

You can run this image with the following command and verify that the added files are in fact there.

```
$ docker run -t -i ouruser/aspnet:v2 /bin/bash
```

##Remove an image from the host

You can remove any images from your host as desired using the `docker rmi` command.

In the following example we'll remove the `aspnet` image we created before.

```
$ docker rmi ouruser/aspnet
```