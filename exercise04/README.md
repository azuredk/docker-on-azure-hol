# Docker Registry on Azure

We've mentioned [Docker Hub](https://hub.docker.com/) before, so to quickly sum up what it is, the Docker Hub is a public registry 
for storing / hosting images. Because its public it could be used by yourself and others. For public images like Ubuntu that is fine,
but maybe you want a private repository, which contains your own custom images that you don't want to share with the world. Maybe because
they include configuration settings. 

Docker Hub does have a paid service, which allows you to host private images, but if you want or prefer
to host a private registry yourself you can do so with the open source version of the Docker registry.

Version 2 of the Docker registry has settings specific to Azure, which makes it fairly easy to setup up a container on Azure with your
own private registry.

##Preparing Azure storage

Before we run the command to start a registry container in our Azure VM, we need to create a storage account.

Using the Azure CLI we can run the following command to create a new storage account
```
$ azure storage account create -l "North Europe" <storage-account-name>
```
Once its created we need to run the following command to list the keys, as we need one of them when creating the registry.
```
$ azure storage account keys list <storage-account-name>
```
It doesn't matter if its the Primary or Secondary key, so just pick one of them.

##Starting the Registry container

Now we can create the registry using the following command where we insert the name of the newly created storage account, 
as well as the primary or secondary key, which gives the Registry access to the storage account.
```
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account-name>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Once the command exits, you can see the container hosting your private Docker Registry instance by running the `docker ps` command on your host.

Remember that port 5000, which is used for the registry container, isn't really open until you open it on the VM in Azure. 
So if you want to be able to access it through the <machine-name>.cloudapp.net:5000 address you need to open the port.

We can open a port on our Azure virtual machine using the Azure CLI and the following command
```
$ azure vm endpoint create <machine-name> 5000 5000
```

>**NOTE:** Please read the [Configuring Docker Registry](http://docs.docker.com/registry/configuration/) documentation to learn how to secure the registry instance and your images

##Pushing an image to the registry

The default way of pushing and pull from a private registry is the same as with the central Docker Hub. The main difference, though, lies in how you include the url when pushing to a private repository.

First, ensure we have an image locally, which we can use to push to the private repository.
```
$ docker pull ubuntu

# Find the image id that corresponds to the ubuntu repository
$ docker images | grep ubuntu | grep latest
ubuntu  latest  8dbd9e392a96  12 weeks ago  263 MB (virtual 263 MB)

# Tag to create a repository with the full registry location.
# The location becomes a permanent part of the repository name.
$ docker tag 8dbd9e392a96 localhost.localdomain:5000/ubuntu

# Finally, push the new repository to its home location.
docker push localhost.localdomain:5000/ubuntu
```

>**NOTE:** It’s important to note that we’re using a domain containing a “.” here, i.e. localhost.domain. Docker looks for either a “.” (domain separator) or “:” (port separator) 
to learn that the first part of the repository name is a location and not a user name. If you just had localhost without either .localdomain or :5000 (either one would do) 
then Docker would believe that localhost is a username, as in localhost/ubuntu.