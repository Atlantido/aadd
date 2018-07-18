#########################
How to install docker and docker-compose
#########################


The first step is go to official page of Docker, select your operating system
and download Docker Community Edition from Docker Store, to do click
[here](https://www.docker.com/community-edition). You must follow the
instructions to correctly install docker, the installation can be either
graphical or by terminal commands, depends your operating system. Once installed
Docker, the next step is install docker-compose to launch any multi-container.
For this purpose you can install this using that
[guide](https://docs.docker.com/compose/install/). The docker version used on
linux to test this guide was 18.03.1-ce and docker-compose version 1.21.2. If
you are using linux, remember to change the permissions to avoid putting sudo in
the docker commands.

*********
Test if docker container is running
*********

A simple way to test the fiware-docker-infrastructure running on docker is using
the next command:
```
$ docker ps
```
It will show you all docker containers running in this moment and its cointainer
id. Furthermore, you can verify the service from docker container is on, for
instance, Orion. To test Orion version, type this on your navigator:
[http://localhost:1026/version](http://localhost:1026/version)

If all is ok, you should watch information about Orion, like uptime or current
version. One more thing would be see the log of containers so you could see the
service messages, use the following command:
```
$ docker logs {CONTAINER_ID}
```
If there are any errors, the will be displayed.
