#Starting a Container on Azure

Now that we have a virtual machine running with Docker on Azure we can test it out by running a Container. 
We'll use our local Docker client in doing so.

The easiest way to verify that the Docker host is running and functional on Azure is to use [busybox](https://registry.hub.docker.com/_/busybox/) for creating a "Hello World" Container:
```
$  docker run busybox echo hello world
```

>Note: If you're using the VM created using the Azure CLI in Excercise 03 - you will need to add the --tls option right after `docker` 


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

Once you've run the named instance and you want to kill it, you can run the `kill`command:

```cli
$ docker kill machinenginx
```

> If you have not named it you can run `docker ps` to get the assigned name or ID and use it to kill it.

When naming a container that name is allocated for that container, to completely remove it run the following:

```cli
$ docker rm machinenginx
```

The nginx image will by default expose port 80 and 443, because of the `EXPOSE` instruction within the image.
The `-P` flag will have Docker assign random ports to the container, so both port 80 and 443 will be exposed externally.

If you want to specify the public ports directly, so you don't get random public ports assigned you need to run it as follows

```cli
$ docker run --name machinenginx -p 49153:80 -p 49154:443 -d nginx
```

The 49153:80 and 49154:443 syntax is specifying the public port mapped to the private port (public:private).


You can verify that the ports have been mapped by running the `docker ps -a` command. The output should look similar to the following:
```
IMAGE               PORTS
nginx:latest        0.0.0.0:49153->80/tcp, 0.0.0.0:49154->443/tcp
busybox:latest
```

To simplify it all, lets use standard ports (80, 443). By just running without specifying ports, it will use the defaults.

```cli
$ docker run --name machinenginx -p 80:80 -p 443:443 -d nginx
```

One thing to note about this is that these port will not be opened by Azure on the VM itself, so we need to do that as extra step in order to access nginx on port 80.

Using the Azure CLI we can run the following command to open public port 80 to port 49153 on the VM. Docker then ensures that inbound tcp traffic on VM port 49153 is routed to the nginx container.

We will need to log in the Azure CLI, if you're not already logged in:

```cli
$ azure login
```

This will present the following:

```cli
info:    Executing command login
info:    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code {SOME CODE} to authenticate.
```

Follow the instructions and go to [https://aka.ms/devicelogin](https://aka.ms/devicelogin) and enter the code and authenticate.

>The port will probably be something else than 49153 - look at the output from `docker ps -a`command.

The following command will open a port in the firewall

```cli
$ azure network nsg rule create --resource-group {RESOURCE GROUP NAME} --nsg-name {NETWORK SECURITY GROUP NAME}
--name http --protocol tcp --direction inbound --priority 1000  
--destination-port-range 80  
--access allow
```

> If you created the machine using the `docker-machine`command without specifying resource group name use `docker-machine` as
> resource group name and YOUR_VM_NAME and postfixed with `-firewall` for the network security group name.

This should give you something like this:

```cli
info:    Executing command network nsg rule create
warn:    Using default --source-address-prefix *
warn:    Using default --destination-address-prefix *
+ Looking up the network security group "myfirstdockervm2-firewall"
+ Looking up the network security rule "http"
+ Creating a network security rule "http"
data:    Id                              : /subscriptions/{YOUR SUBSCRIPTION ID}/resourceGroups/{RESOURCE GROUP}/providers/Microsoft.Network/networkSecurityGroups/{NETWORK SECURITY GROUP}/securityRules/http
data:    Name                            : http
data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
data:    Provisioning state              : Succeeded
data:    Source IP                       : *
data:    Source Port                     : *
data:    Destination IP                  : *
data:    Destination Port                : 80
data:    Protocol                        : Tcp
data:    Direction                       : Inbound
data:    Access                          : Allow
data:    Priority                        : 1000
info:    network nsg rule create command OK
```

We need the public IP address to be able to test that everything is working, you can get it from the portal or use the Azure CLI as well.

```cli
$ azure vm list-ip-address -g {RESOURCE GROUP}
```

It should yield the following:

```cli
info:    Executing command vm list-ip-address
+ Getting virtual machines
+ Looking up the NIC "{VM NAME}-nic"
+ Looking up the public ip "{VM NAME}-ip"
data:    Resource Group   Name              Public IP Address
data:    ---------------  ----------------  -----------------
data:    {RESOURCE GROUP} {VM NAME}         {IP ADDRESS}
info:    vm list-ip-address command OK
```

Open a browser and go to `http://{IP ADDRESS}` and you should see the nginx welcome page.

That's it for now. You can of course stop and remove these containers and images as we won't be using them again.