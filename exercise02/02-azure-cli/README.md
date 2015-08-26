#Azure CLI

Microsoft Azure Xplat-CLI for Windows, Mac and Linux - or Azure CLI for short is a cross-platform command line interface
based on NodeJs. The Azure CLI provides much of the same functionality found in the Azure Management Portal, such as the 
ability to manage websites, virtual machines, mobile services, SQL Database and other services provided by the Azure platform.

>The Azure CLI is written in JavaScript, and requires Node.js. It is implemented using the Azure SDK for Node.js, and released 
under an Apache 2.0 license. The project repository is located at https://github.com/azure/azure-xplat-cli.

##Installing the Azure CLI

There are three ways of installing the Azure CLI, and we'll cover two of those in this section. 
The third option is to use a Docker container, which is covered in the next section.

First option is to use an installer, which is available for Windows, Mac and Linux.

* [Windows installer](http://go.microsoft.com/?linkid=9828653&clcid=0x406)
* [Mac OS X installer](http://go.microsoft.com/fwlink/?linkid=252249&clcid=0x406)
* [Linux](http://go.microsoft.com/fwlink/?linkid=253472&clcid=0x406)

Second option is to use NodeJs and NPM.

From a command line simply write `npm install azure-cli -g` (assuming you have NodeJs installed already).
If not then go to [nodejs.org/download](https://nodejs.org/download/) and download the Windoes installer.

##Using Docker Container

The third option of installing the Azure CLI is to use a Docker container. If you created a private lab with Ubuntu and Docker 
in a virtual machine this might be the easiest way of getting the CLI up and running. 
But even if you creatd a VM with Docker on Azure, you can also use this approach for installing the Azure CLI as a Container.

In a Docker host run the following command `docker run -it microsoft/azure-cli`. 