# Docker on Azure Hands-on Lab

This workbook contains exercises for getting started with running Docker on Azure. 
Following the exercises you will learn how to setup your local environment with your own private
Docker lab by installing Ubuntu on a Virtual Machine using Hyper-V or VirtualBox, you'll get 
familiar with Docker by trying out different commands, installing containers and creating 
Dockerfiles for your own images. You will also learn how to setup a private Docker registry 
hosted on Azure and how to deploy one or more containers, also, running on Azure.

## Prerequisites
Before we get started you need to install a few things locally in order to create your own
private lab and in order to interact with remote resources on Azure.

We recommend installing and configuring the 3 prerequisites as mentioned in the tutorials below in order to get a local setup
that is consistent with what is used in the exercises, and to get a client environment that is fairly close to what you would have on a linux machine (even though we are running Windows).

* [Prerequisites](prerequisites/README.md)
	* [Installing Git](prerequisites/git/README.md)
	* [Installing Cmder](prerequisites/cmder/README.md)
	* [Getting the Docker binary](prerequisites/docker/README.md)

##Private Lab

To get a better understanding of working with Docker and Linux (Ubuntu specifically) we highly recommend
starting with setting up a private lab. Here you will install Ubuntu server in a virtual machine on your local machine.
And you will learn to install and update the Docker daemon within Ubuntu.

The private lab is not necessary to complete the exercises below, so please consider this step optional. 
You might consider skipping or including this part based on the time available to complete this hands-on lab.

* [Getting Started](privatelab/README.md)
	* [Setting up Hyper-V](privatelab/01-hyperv/README.md)
	* [Setting up VirtualBox](privatelab/01-virtualbox/README.md)
	* [Installing Ubuntu](privatelab/02-ubuntu/README.md)
	* [Installing Docker in Ubuntu](privatelab/03-docker/README.md)

## Exercises
Once you have the prerequisites in order you can follow the exercises outlined below.

Please note that certain exercises have two routes you can take depending on, which fits best
with the system that you are running your local lab on (ie. running Mac OS, Windows 8 or Windows 10).

* [Docker on Azure](exercise02/README.md)
	* [Docker Machine](exercise02/01-machine/README.md)
	* [Azure CLI](exercise02/02-azure-cli/README.md)
	* [Using the Azure CLI](exercise02/03-using-the-azure-cli/README.md)
	* [Starting a Container on Azure](exercise02/04-azure-container/README.md)
* [Getting familiar with Docker](exercise03/README.md)
	* [Containers](exercise03/01-containers/README.md)
	* [Images](exercise03/02-images/README.md)
	* [Dockerfile](exercise03/03-dockerfile/README.md)
* [Docker Registry on Azure](exercise04/README.md)
* [ASP.NET 5 beta6 Container on Azure](exercise05/README.md)
* [Visual Studio 2015 Docker extension](exercise06/README.md)
* [Docker Compose on Azure](exercise07/README.md)
* [Azure Resource Manager Docker templates](exercise09/README.md)