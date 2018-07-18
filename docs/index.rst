############################
FIWARE-Docker-infrastructure
############################

This repository contains a easy way to deploy the FIWARE IoT Stack through docker compose

************
Architecture
************

High-level architecture
=======================

Docker Architecture
===================

All the services share the subnet and will be reachable through the dns namespaces provided by docker.

****
API
****

Postman collections covering the main functionalities of each of the services composing the architecture is provided.
There are services not offering a API to interact with or providing a user interface, for this reasons the postman
collection do not cover all the services deployed.

************
Dependencies
************
  * docker (18.03.1-ce)
  * docker-compose (1.21.2)

*************
Configuration
*************

All the services in the architecture are deployed using a docker-compose file therefore the configuration options
are explicitly declared in this file. For services with complex configuration a brief extra description is provided.
 
IoT Agent configuration
=======================

In order to be able to map the OMA LWM2M information model to OMA NGSI entities, atributes and metadata a configuration
file is created reflecting the correspondence. **config.js** in the docker-compose/ directory contains two blocks:
- LwM2M server configuration, specifying aspects like server port, content-format used or the log level of the service.
- NGSI configuration, where information about the http server, the storage and the mapping between the protocols are
specified. On the other hand, a dynamic configuration can be carried out using the service API. The postman collection of this service
provides a skeleton template.

More information about the component can be found in the [LWM2M IoT Agent Guide](http://fiware-iotagent-lwm2m.readthedocs.io/en/latest/).

Cygnus configuration
====================
**agent.conf** In order to configure the channels and databases in which the information will persist,
               it is necessary to configure the agent.conf file in the docker-compose/ directory.
               This file will be loaded into the docker container as a configuration file.
               An example of the file that is loaded by default can be found in this [url](https://github.com/telefonicaid/fiware-cygnus/blob/master/docker/cygnus-ngsi/agent.conf).
               In the previous example we can see how to initialize each of the different connectors to databases.
               <br><b>_To simplify the debug better add only the necessary ones._</b>

**Orion-Cygnus Communication** In order to get the information that reaches Orion to be persistent, it is necessary to
create subscriptions on Orion by setting Cygnus as the url of the callback. An example of the subscription to create can
be found in the postman collection within the main directory.

More information about the component can be found in the [Cygnus Guide](http://fiware-cygnus.readthedocs.io/en/latest/).

QuantumLeap, Crate and Grafana configuration
============================================

These three components work jointly to accomplish a visual representation of the information in Orion Context Broker.
QuantumLeap is a library that receives Orion information through subcriptions and stores the information in a Crate database.
Last, the grafana container launchs a web services with user interface in which the Crate database deployed can be configured
as data source. A more detailed guide of this services interaction can be found in the use-cases/ directory.

Perseo-core and Perseo-fe configuration
=======================================

Perseo CEP is a Complex Event Processing (CEP) module. In this module, Perseo-core is the back-end of Perseo CEP, the rule-engine. It checks incoming events and, if any action must be done, it call to Perseo-fe through a POST request. Perseo-fe refresh the set of Perseo-core rule periodically. When Perseo-core send an action to Perseo-fe, it is responsible of send an action vía SMS, e-mail or HTTP. A more detailed guide of this services interaction can be found in the use-cases/ directory.
**docker-compose.yml** In order for Perseo CEP can send a notification, it must have configured the following servers: SMPP, SMTP and HTTP in docker-compose.yml file contained in the docker-compose/ directory. The environment variables available for Perseo configuration can be foun in this [url](https://github.com/telefonicaid/perseo-fe/blob/master/documentation/configuration.md#environment-variables-configuration).

****************************************
How to install docker and docker-compose
****************************************

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

Test if docker container is running
===================================

A simple way to test the fiware-docker-infrastructure running on docker is using
the next command:

.. code::shell

  $ docker ps

It will show you all docker containers running in this moment and its cointainer
id. Furthermore, you can verify the service from docker container is on, for
instance, Orion. To test Orion version, type this on your navigator:
[http://localhost:1026/version](http://localhost:1026/version)

If all is ok, you should watch information about Orion, like uptime or current
version. One more thing would be see the log of containers so you could see the
service messages, use the following command:

.. code::shell

  $ docker logs {CONTAINER_ID}

If there are any errors, the will be displayed.

*********************
Build, deploy and run
*********************

The architecture building and execution must contains using docker-compose must contains a previous step, the IoT Agent
container building. This is due to the LWM2M IoT Agent madurity state. Currently some modifications in its code are needed
and, for this reason, the modified source code is provided to ensure the interopeability between devices and Orion.

Build LWM2M IoT Agent
=====================

Within the IoT Agent folder execute:

.. code::shell

  $ docker build -t "iotagent:latest" $(pwd) 

In order for the iotagent to receive its configuration, a config.js
file must exist in the docker-compose directory. This directory will
include the information that must be mapped between the devices and
the orion.

Build architecture through docker-compose
=========================================

Launch infrastructure:

.. code::shell

  $ docker-compose up

Launch infrastructure in background:

.. code::shell

  $ docker-compose up -d

Stop infrastructure:
.. code::shell

  $ docker-compose down

How to install Postman
======================

Postman is a tool to realize multiple request to any REST API, for instance,
Orion REST API. Permit automate functions like obtain entities of Orion, create
new entity or delete entity. To download Postman click
[here](https://www.getpostman.com/apps) and select you operating system.
Furthermore, you can install a version of chrome web store.
