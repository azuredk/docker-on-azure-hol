#Azure Resource Manager Docker templates

We've briefly mentioned the new way of deploying / provisioning services on Azure - using Azure Resource Manager.

There are a few different ways of deploying Azure resources to a Resource Group. One way is using prebuilt templates, which can be found on github under the 
[Azure Resource Manager QuickStart Templates](https://github.com/Azure/azure-quickstart-templates) repository. You can use them by clicking the "Deploy to Azure" button or through the Azure CLI.

Related to Docker on Azure there are a few templates of interest:
* [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [Simple Docker Swarm Cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-swarm-cluster-simple)
* [Docker Swarm Cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-swarm-cluster)

There are usually 3 steps in deploying these templates using the Azure CLI
1. Find the desired template and inspect the parameters that are needed to deploy it.
2. Create a resource group in a chosen location.
3. Create a deployment using the template, and entering the parameters when prompted for these.

First, make sure to switch the Azure CLI to the `arm` mode
```
$ azure config mode arm
```

Next, create a resource group within a given location
```
$ azure group create myResourceGroup westus
```

Deploy a template by providing a url to the template and the group to deploy it to
```
$ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json myResourceGroup firstDeployment
```

More details about using the Azure CLI to deploy templates can be found here [Deploy and Manage Virtual Machines using Azure Resource Manager Templates and the Azure CLI](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-deploy-rmtemplates-azure-cli/).