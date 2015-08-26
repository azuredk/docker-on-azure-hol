#Docker Compose on Azure

Compose (previously known as Fig) is a tool for defining and running multi-container applications with Docker.

>_Compose is great for development environments, staging servers, and CI. We don’t recommend that you use it in production yet._

Using compose is a 3 step process, where you start off by defining a `Dockerfile` for your app's environment, then use `docker-compose.yml` to define the services that your app will be using, so they can be run together in an isolated environemnt.
Lastly, run `docker-compose up` and Compose will start and run your entire app

But first we need to install Compose.

> **Note:** Using the Docker VM extension on Azure will actually ensure that Compose is also installed. You can verify whether its installed by typing the following command `docker-compose --version`.

##Installing Compose

In order to install Compose on an Azure VM you need to SSH into the VM and run the following two commands
```
$ curl -L https://github.com/docker/compose/releases/download/1.4.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

$ chmod +x /usr/local/bin/docker-compose
```

>For details on how to SSH into an Azure Ubuntu VM from Windows please refer to these tutorials: [How to Log on to a Virtual Machine Running Linux](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-how-to-log-on/) and [How to Use SSH with Linux on Azure](https://azure.microsoft.com/da-dk/documentation/articles/virtual-machines-linux-use-ssh-key/) 

##Create a docker-compose.yml configuration file

In order to run a multi-container app we need to create a `docker-compose.yml` file and specify the images that should be used.

For simplicity we'll use two images that exists on Docker Hub.

The `docker-compose.yml` file that we'll use for this exercise will specify Wordpress as the web app, and MariaDb (MySQL) as its Database.
```
 wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 8080:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

Create a folder within your Azure VM and place the `docker-compose.yml` file there with the above content.

To edit a file with vim through the Ubuntu commandline use the following command
```
vim docker-compose.yml
```
To save the file and exit vim use the following command
```
Press Esc then :w or :wqor :x to save and exit

#Leave without saving
Esc then :q!
```

>_For details on yml file syntax, see [docker-compose.yml](http://docs.docker.com/compose/yml/) reference._

##Starting the Containers with Compose

From within the folder you created above type the following command
```
$ docker-compose up -d
```
Be sure to use the `-d` flag on start-up so that the containers run in the background continuously

You can verify that the containers are running using the `docker-compose ps` command.

Using the Azure CLI we'll open port 80 on the VM, so we can access the Wordpress container through our browser
```
$ azure vm endpoint create <machine-name> 80 8080
```
Now, opening the machine-name.cloudapp.net url in a browser should show the Wordpress install screen.