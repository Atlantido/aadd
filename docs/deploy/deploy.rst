Build, deploy and run
===============

The architecture building and execution must contains using docker-compose must contains a previous step, the IoT Agent
container building. This is due to the LWM2M IoT Agent madurity state. Currently some modifications in its code are needed
and, for this reason, the modified source code is provided to ensure the interopeability between devices and Orion.

Build LWM2M IoT Agent
---------------


Within the IoT Agent folder execute:

```
$ docker build -t "iotagent:latest" $(pwd)
```

In order for the iotagent to receive its configuration, a config.js
file must exist in the docker-compose directory. This directory will
include the information that must be mapped between the devices and
the orion.

Build architecture through docker-compose
---------------


Launch infrastructure:

```
$ docker-compose up
```

Launch infrastructure in background:
```
$ docker-compose up -d
```

Stop infrastructure:
```
$ docker-compose down
```
How to install Postman
---------------

Postman is a tool to realize multiple request to any REST API, for instance,
Orion REST API. Permit automate functions like obtain entities of Orion, create
new entity or delete entity. To download Postman click
[here](https://www.getpostman.com/apps) and select you operating system.
Furthermore, you can install a version of chrome web store.
