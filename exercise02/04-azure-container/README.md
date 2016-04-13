#Starting a Container on Azure

Now that we have a virtual machine running with Docker on Azure we can test it out by running a Container. 
We'll use our local Docker client in doing so.

The easiest way to verify that the Docker host is running and functional on Azure is to use [busybox](https://registry.hub.docker.com/_/busybox/) for creating a "Hello World" Container:
```
$  docker run busybox echo hello world
```
The output will look something like this
```
    Unable to find image 'busybox:latest' locally
    511136ea3c5a: Pull complete
    df7546f9f060: Pull complete
    ea13149945cb: Pull complete
    4986bf8c1536: Pull complete
    busybox:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for busybox:latest
    hello world
```

Let's try running another container that might be a bit more useful.

Running the following command will start an [nginx](https://hub.docker.com/_/nginx/) server
```
$ docker run --name machinenginx -P -d nginx
```
The `-d` flag will ensure that the container runs in the background continuously.

The nginx image will by default expose port 80 and 443, because of the `EXPOSE` instruction within the image.
The `-P` flag will have Docker assign random ports to the container, so both port 80 and 443 will be exposed externally.

You can verify that the ports have been mapped by running the `docker ps -a` command. The output should look similar to the following:
```
IMAGE               PORTS
nginx:latest        0.0.0.0:49153->80/tcp, 0.0.0.0:49154->443/tcp
busybox:latest
```
One thing to note about this is that these port will not be opened by Azure on the VM itself, so we need to do that as extra step in order to access nginx on port 80.

Using the Azure CLI we can run the following command to open public port 80 to port 49153 on the VM. Docker then ensures that inbound tcp traffic on VM port 49153 is routed to the nginx container.
>The port will probably be something else than 49153 - look at the output from `docker ps -a`command.

```
$ azure vm endpoint create machine-name 80 49153
info:    Executing command vm endpoint create
+ Getting virtual machines
+ Reading network configuration
+ Updating network configuration
info:    vm endpoint create command OK
```
Open a browser and go to machine-name.cloudapp.net url and you should see the nginx welcome page.

That's it for now. You can of course stop and remove these containers and images as we won't be using them again.