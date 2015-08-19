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

* [Prerequisites](prerequisites/README.md)
	* [Installing Git](prerequisites/git/README.md)
	* [Installing Cmder](prerequisites/cmder/README.md)
	* [Getting the Docker binary](prerequisites/docker/README.md)

## Exercises
Once you have the prerequisites in order you can follow the exercises outlined below.

Please note that certain exercises have two routes you can take depending on, which fits best
with the system that you are running your local lab on (ie. running Mac OS, Windows 8 or Windows 10).

* [Getting Started](exercise01/README.md)
	* [Setting up Hyper-V](exercise01/01-hyperv/README.md)
	* [Setting up VirtualBox](exercise01/01-virtualbox/README.md)
	* [Installing Ubuntu](exercise01/02-ubuntu/README.md)
	* [Installing Docker in Ubuntu](exercise01/03-docker/README.md)
* [Getting familiar with Docker](exercise02/README.md)
	* [Containers](exercise02/01-containers/README.md)
	* [Images](exercise02/02-images/README.md)
	* [Dockerfile](exercise02/03-dockerfile/README.md)
* [Docker on Azure](exercise03/README.md)
	* [Docker Machine](exercise03/01-machine/README.md)
	* [Azure CLI](exercise03/02-azure-cli/README.md)
	* [Using the Azure CLI](exercise03/03-using-the-azure-cli/README.md)
	* [Starting a Container on Azure](exercise03/04-azure-container/README.md)
* [Docker Registry on Azure](exercise04/README.md)
* [ASP.NET 5 beta6 Container on Azure](exercise05/README.md)
* [Visual Studio 2015 Docker extension](exercise06/README.md)
* [Azure Resource Manager Docker templates](exercise07/README.md)