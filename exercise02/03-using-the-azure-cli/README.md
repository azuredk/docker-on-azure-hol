#Using the Azure CLI

With the Azure CLI installed on our local machine or as a Docker container we can start using it to create virtual machines on Azure.

First, we need to authenticate against Azure so we can create virtual machines under the right subscription.

To log in from the Azure CLI using a work or school account, use the following command:
`azure login -u <username>` and enter your password when prompted for.

>**NOTE:** If this is the first time you have logged in with these credentials, you will receive a 
prompt asking you to verify that you wish to cache an authentication token. 
This prompt will also occur if you have previously used the `azure logout` command described below. 
To bypass this prompt for automation scenarios, use the `-q` parameter with the `azure login` command.

To log out, use the following command: `azure logout -u <username>`

This login option only works for Microsoft organizational accounts and service principals, so if your login fails you'll need to set your account in a different way.
By just using `azure login` without the option of a username, you'll be asked to navigate to (devicelogin)[http://aka.ms/devicelogin) and enter a one-time-password.

Run the following commands to first download a `publishingsettings` file for your azure account, and then import it.
```
$ azure account download

#Import the downloaded file
$ azure account import my-azure-account.publishingsettings
```
Please keep in mind that the download command will open a browser and download the active account from Azure. If you are already signed into the Azure portal it will be downloaded right away, otherwise you need to sign in first.
So its a good idea to ensure that the correct account is active before issueing the download command.

Also note that the import command will take the file from the current folder unless you specify an absolute path.

Once you have imported the `publishingsettings` file you should ensure that the correct subscription is selected, so follow the instructions in the following section to set the correct subscription.


##Commands

Most commands are formatted as azure `<command> <operation> [parameters]` and perform an operation on a service or object such 
as your account configuration. Other commands provide sub-commands and follow the format 
`azure <command> <subcommand> <operation> [parameters]`.

To view subscriptions you have available from the CLI type the following command:
```
$ azure account list
```

If you have multiple subscriptions available, you can use the following command to switch between them and set a new default.
```
$ azure account set <subscription guid>
```

You can explorer the options under each of the commands by simply typing `azure <command>` like so:
```
# List subcommands for the primary vm command
$ azure vm
```
If any of the commands you type from the Azure CLI fails or exits with an error you can add the `-vv` option to the command to get detailed output from the REST calls to Azure
```
$ azure site create --location "North Europe" mytestsite -vv
```

##Creating a virtual machine with Docker

Now that we have the Azure CLI in place, and we've learned a bit more about the commands we can now create a virtual machine on Azure
based on an Ubuntu image with the Docker daemon pre-installed.

First, we'll find the available Ubuntu images as we'll need to specify the name of the image when creating the virtual machine.
Use the following command to list VM images containing the word "Ubuntu-14_04":
```
$ azure vm image list | grep Ubuntu-14_04
```
Based on availability, updates or the like the list might change, but given a machine name `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140724-en-us-30GB`
we could type the following command in order to create a virtual machine based on an Ubuntu image with Docker enabled.
```
$ azure vm docker create <machine-name> -l "North Europe" -e 22 "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_3-LTS-amd64-server-20150805-en-us-30GB" <username> <password>
```
The `-l` flag specifies the region that our VM will be created within, and the `-e` flag specifies the port.

To test that the newly created virtual machine works as expected, type the following command from the Docker client command prompt:
```
$ docker --tls -H tcp://<machine-name>.cloudapp.net:2376 info
```

##Docker Host VM Authentication
In addition to creating the virtual machine with Docker installed, the `azure vm docker create` command also automatically creates 
the necessary certificates to allow your Docker client to connect to the Azure container host using HTTPS.Tthe certificates are stored 
on both the client and host machines. On subsequent runs, the existing certificates are reused and shared with the new host.

By default, certificates are placed in `~/.docker`, and Docker will be configured to run on port **2376**. If you would like to use a 
different port or directory, then you can use one of the following `azure vm docker create` command-line options to configure your 
Docker container host VM to use a different port or different certificates for connecting clients:
```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

>The Docker daemon on the host is configured to listen for and authenticate client connections on the specified port using 
the certificates generated by the `azure vm docker create` command. The client machine must have these certificates to gain 
access to the Docker host.

##Connecting to a Docker host

Above we used the following command to connect to Docker in our Azure VM
```
$ docker --tls -H tcp://<machine-name>.cloudapp.net:2376 info
```
The `-H` flag tells our Docker client, which host to connect to. So you could manage multiple hosts from the same client, but if you use the same host over and over again you might want to reduce the amount of typing by using the `DOCKER_HOST` environment variable.

To set a specific host use the following command
```
export DOCKER_HOST="tcp://<machine-name>.cloudapp.net:2376
```
Then you should be able to write `docker --tls info` to access the same machine. And you can always switch the Docker host in the environment variable.

##Additional resources

One thing to be aware of is the difference between using Service Management (v1) and the new Resource Management (v2).
Resource Management (ARM) is the way of the future, so its recommended to get familiar with that. More details can be found in the links below.
But the Azure CLI defaults to Azure Service Management mode. You can switch between these two modes with the following comands:

```
#Switch to ARM
$ azure config mode arm

#Switch to back to ASM
$ azure config mode asm
```

* [Using the Azure CLI with the Service Management (or ASM mode) commands](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-command-line-tools/)
* [Using the Azure CLI with the Resource Management (or ARM mode) commands](https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/)