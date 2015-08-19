#Visual Studio 2015 Docker extension

If you have Visual Studio 2015 installed you can utilize the new Docker extension, which is available in the [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/6f638067-027d-4817-bcc7-aa94163338f0)

From the Visual Studio Gallery the extension is described as follows: 
>The Visual Studio 2015 Tools for Docker Preview enables developers to build and publish an ASP.NET 5 Web or console application to a Docker container running on a Linux virtual machine.

So basically, you just install the extension, create a new ASP.NET 5 site using the built-in template.
Right-click Publish to get the publishing dialog, which allows us to pick Docker as the way to Publish.

![](PublishMenu.png)

![](TargetDocker.png)

If you want to use an existing virtual machine you can choose one after having signed into your Azure account.

![](ChooseExistingVM.png)

Or you can create a new virtual machine with the desired configuration

![](CreateVirtualMachine.png)

Once you have finished the configuration of the "Publish to Docker" you can publish your web app, and follow along in the build output.

There is really not more to it. But of course its a different way of publishing a web app or console app to Docker.