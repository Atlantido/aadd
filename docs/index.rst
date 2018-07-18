FIWARE-Docker-infrastructure
===============

Introduction
---------------

This repository contains a easy way to deploy the FIWARE IoT Stack through docker compose

Architecture
---------------

High-level architecture
~~~~~~~~~~~~~~~~~~~

<br>



<br>

Docker Architecture
~~~~~~~~~~~~~~~~~~~

All the services share the subnet and will be reachable through the dns namespaces provided by docker.



API
---------------

Postman collections covering the main functionalities of each of the services composing the architecture is provided.
There are services not offering a API to interact with or providing a user interface, for this reasons the postman
collection do not cover all the services deployed.


Dependencies
---------------

  * docker (18.03.1-ce)
  * docker-compose (1.21.2)


Build, deploy and run
---------------

The architecture building and execution must contains using docker-compose must contains a previous step, the IoT Agent
container building. This is due to the LWM2M IoT Agent madurity state. Currently some modifications in its code are needed
and, for this reason, the modified source code is provided to ensure the interopeability between devices and Orion.

#### Build LWM2M IoT Agent

Within the IoT Agent folder execute:

```
$ docker build -t "iotagent:latest" $(pwd)
```

In order for the iotagent to receive its configuration, a config.js
file must exist in the docker-compose directory. This directory will
include the information that must be mapped between the devices and
the orion.

#### Build architecture through docker-compose

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

How to use orion Context Broker
---------------

All subsections use Postman to realise requests and the version of Orion used is
version 2.

How to create an entity
~~~~~~~~~~~~~~~~~~~

Is needed a HTTP POST message to create an entity in Orion. The body message
needs the entity representation in JSON format. And the headers message must
have “fiware-service”, “fiware-servicepath” and “Content-Type”. Orion use
“fiware-service” and “fiware-servicepath” for save the entity.

The following image shows the headers fields of the http POST message:


The params with {{name-param}} must be changed for a value and it mean the
following:
- {{url}}: URL where Orion is located.
- {{port-orion}}: Port to connect with Orion Context Broker.
 - {{fiware-service}}: Value for the key fiware-service.
  - {{fiware-servicepath}}: Value for the key fiware-servicepath.

The following image shows the body message in JSON format:
```
{
 "id": "test",
 "type": "SmartSpot",
  "CO": {
           "type": "Number",
           "value": "9.626428598980013",
           "metadata": {}
       },
  "location": {
           "type": "Point",
           "value": {
               "latitude" : "145",
               "longitude" : "512"
           },
           "metadata": {}
       }
}
```
If the request has been successful then Orion response will return 201 as status
code else Orion will return 422 as status code if exist an entity with same
id or will return 400 as status code if the body message is not in the correct
format.

How to retrieve an entity
~~~~~~~~~~~~~~~~~~~

Is needed a HTTP GET method for retrieve an entity from Orion. The URL must have
the entity id and the headers must have the fields “fiware-service” and “fiware-
servicepath” to indicate to Orion where to search for the entity.
The next image show an example of message for retrieve an entity:


The params with {{name-param}} must be changed for a value and it mean the
following:
- {{url}}: URL where Orion is located.
- {{port-orion}}: Port to connect with Orion Context Broker.
- {{fiware-service}}: Value for the key fiware-service.
- {{fiware-servicepath}}: Value for the key fiware-servicepath.
- {{fiware-entity}}: Entity id to retrieve.

If exists an entity with same id in fiware-service and fiware-servicepath then
Orion will return 200 as status code and the information of the entity else
Orion will return 404 with status code.

FIWARE datamodels
~~~~~~~~~~~~~~~~~~~

Fiware datamodels are standars to represent devices in the Fiware architecture.
They are used with FIWARE NGSI version 2.

You can find more information of the diferents FIWARE datamodels in the
following URL: https://www.fiware.org/developers/data-models/

Retrieve an entity with FIWARE datamodel
---------------

Is needed a HTTP GET method for retrieve an entity from Orion. The URL must have
the entity id and the headers must have the fields “fiware-service” and “fiware-
servicepath” to indicate to Orion where to search for the entity and a query
param for indicate to Orion to show the information of the entity in datamodel
format.
The following image shows an example of message for retrieve an entity with
query param:

The params with {{name-param}} must be changed for a value and it mean the
following:
- {{url}}: URL where Orion is located.
- {{port-orion}}: Port to connect with Orion Context Broker.
- {{fiware-service}}: Value for the key fiware-service.
- {{fiware-servicepath}}: Value for the key fiware-servicepath.
- {{fiware-entity}}: Entity id to retrieve.

If exists an entity with same id in fiware-service and fiware-servicepath then
Orion will return 200 as status code and the information of the entity in
dataModel format else Orion will return 404 with status code.

How to delete an entity
~~~~~~~~~~~~~~~~~~~

Is needed a HTTP DELETE method to delete an entity from Orion. The URL must have
the entity id and the headers must have the fields “fiware-service” and “fiware-
servicepath” to indicate to Orion where to search for the entity to delete it.
The next image show an example of message for delete an entity:

The params with {{name-param}} must be changed for a value and it mean the
following:
- {{url}}: URL where Orion is located.
- {{port-orion}}: Port to connect with Orion Context Broker.
- {{fiware-service}}: Value for the key fiware-service.
- {{fiware-servicepath}}: Value for the key fiware-servicepath.
- {{fiware-entity}}: Entity id to retrieve.

If exists an entity with same id in fiware-service and fiware-servicepath then
Orion will return 204 as status code else Orion will return 404 with status
code.

Compatibility and versions
---------------

The docker-compose service description do not use version tag in the majority of the services. That implies the use of
the service last version at the time of perform the pull.
There are some exceptions like the MongoDB version or the IoT Agent that must be build in the host machine.

To Be Done
---------------

* Add FIWARE security stack
* Create different docker-composes adapting the components to the user case required.
* Change diagrams and add perseo documentation

Extensibility
---------------

From HOP Ubiquitous we are pretty interested in the extension of the services ready to be deployed. If you have experience
with any other FIWARE component do not hesitate to contact us.

#### Mantainers
- german@hopu.eu
- joseluis@hopu.eu
- felipe@hopu.eu

Known issues
---------------

* Currently a IoT Agent memory issue has been discovered. The error implies the service stop and for this reason a restart
condition is provided in the docker-compose file.

---
***
<p align="center">
<img src="http://www.hopu.eu/wp-content/uploads/2016/05/HOP_LOGO_NEW_LINE.png" width=300/>
<img src="http://www.fiware4industry.com/wp-content/uploads/2015/11/FIWARE-for-Industry.png" width=300/>
<p>
