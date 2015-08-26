# ASP.NET 6 beta6 Container on Azure

Back in exercise 3 part 3 we looked at the Dockerfile for the microsoft/aspnet image, and how it was built up. In this exercise we'll
build our own image around a sample ASP.NET 5 beta6 site, and deploy that to a container.

First off we need a site. If you have Visual Studio 2015 and DNX (currently beta 6) installed then you can simply create a site using the default template.
If you don't have Visual Studio and DNX installed you can use the sample site avaiable in this [github repository](https://github.com/azuredk/HelloMvc).

>*For simplicity it is recommended to simply clone the above mentioned repository*

Cloning the repository from within your virtual machine:
```
$ git clone https://github.com/azuredk/HelloMvc.git
```

##Building the Dockerfile

If you have `docker` available in your PATH, which ideally you should have, then you can build the Dockerfile that came with the HelloMvc site.
So inside the `HelloMvc` folder (or simply within the HelloMvc folder if you prefer) type the following command
```
$ docker build -t hellomvc .
```
When its done building, which can take a bit of time, you should see a message that starts with "Successfully built".
If you see a message like "The command 'dnu restore' returned a non-zero code: 1" it usually means that some nuget packages could not be downloaded and the image thus failed to build.

##Run the container

Hopefully the image builds successfully, so you can start it using the following command:
```
$ docker run -t -d -p 80:5004 hellomvc
```
The `-t` flag attaches a pseudo-tty to the container, and the `-d` flag runs the container in the background, so our console isn't attached to our development machine's shell.
The `-p` flag maps port 80 of the VM to port 5004 of the container. Remember that these ports usually has to be opened when dealing with an Azure VM.

##Pushing the image to our private registry

Once the image is built we can push it to our private repository using the following command (using the details from the previous exercise):
```
$ docker push my.registry.address:port/repositoryname
#Example: localhost.localdomain:5000/ubuntu
```

On our Azure virtual machine we should now be able to pull our custom ASP.NET 5 image from the private registry using the following command
```
$ docker pull my.registry.address:port/repositoryname
```