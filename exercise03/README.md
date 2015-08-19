# Docker on Azure

In the previous exercises we've learned a lot about using Docker, we've installed Ubuntu and the Docker daemon locally, so now its time to create a remote host. 
We will use this host (Ubuntu VM with Docker on Azure) for a private Docker registry, which we'll create in the next exercise (exercise number 4).

In this exercise we'll focus on two different ways of creating a virtual machine on Azure with Docker already installed.

First we'll look into using Docker Machine with Azure and then we'll try to install and use the Azure CLI to create a virtual machine with Docker. 
You can choose to install the Azure CLI on your local machine (please note that NodeJs is required for the CLI to run) or you can simply create a container with the CLI in our existing Ubuntu VM. 

* [Docker Machine](01-machine/README.md)
* [Azure CLI](02-azure-cli/README.md)
* [Using the Azure CLI](03-using-the-azure-cli/README.md)
* [Starting a Container on Azure](04-azure-container/README.md)