#Getting and configuring Docker binaries

In order to issue commands against remote Docker hosts (running the Docker daemon/engine) you will need to have a Docker client locally. 
This is a single executable file, which doesn't contain the Docker engine, it only acts as a client.

The Windows [Docker binaries](https://docs.docker.com/installation/binaries/) can be downloaded using the following URL pattern:

```
https://get.docker.com/builds/Windows/i386/docker-latest.exe

https://get.docker.com/builds/Windows/x86_64/docker-latest.exe
```

If you want a specific version of the Docker client for Windows you can insert the version number instead of "-latest" like so:

```
Version 1.8.1:
https://get.docker.com/builds/Windows/i386/docker-1.8.1.exe

64bit version of 1.8.1
https://get.docker.com/builds/Windows/x86_64/docker-1.8.1.exe
```

Also download **Docker Machine**, which will be used some of the exercises. You can find the Docker Machine binaries on the github page for Machine [https://github.com/docker/machine/releases](https://github.com/docker/machine/releases).

Once downloaded check that the file isn't Blocked, then rename it to Docker.exe (rename the Docker Machine binary to docker-machine.exe) and place it in a known location like `C:\Docker`.
Then add it to the Windows PATH by adding `;C:\Docker` to the existing PATH, which you can find by going to Control Panel > System > Advanced System Settings
Advanced (tab) > Environment Variables (button) > System Variables

![](windows-path.png) 

Alternatively, run the following command from your Command Line Prompt (cmd.exe):
```
set PATH=%PATH%;"c:\Docker"
```