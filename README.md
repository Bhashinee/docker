# Ballerina Docker Extension
 
Annotation based docker extension implementation for ballerina. 

[![Build Status](https://wso2.org/jenkins/job/ballerinax/job/docker/badge/icon)](https://wso2.org/jenkins/job/ballerinax/job/docker/)
[![Build Status](https://img.shields.io/travis/ballerinax/docker.svg?logo=travis)](https://travis-ci.org/ballerinax/docker)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 
## Features:
- Dockerfile generation based on @docker:Config annotations. 
- Docker image generation. 
- Docker push support with docker registry.
- Docker based ballerina debug support. 
- Copy file support. 

## Supported Annotations:

### @docker:Config{}
- Supported with ballerina services or listeners.

|**Annotation Name**|**Description**|**Default value**|
|--|--|--|
|name|Name of the docker image|output balx file name|
|registry|Docker registry|None|
|tag|Docker image tag|latest|
|buildImage|Whether to build docker image|true|
|dockerHost|Docker host IP and docker PORT. ( e.g minikube IP and docker PORT)|unix:///var/run/docker.sock|
|dockerCertPath|Docker cert path|null|
|baseImage|Base image to create the docker image|ballerina/ballerina-runtime:latest|
|enableDebug|Enable debug for ballerina|false|
|debugPort|Remote debug port|5005|
|push|Push to remote registry|false|
|username|Username for docker registry|None|
|password|Password for docker registry|None|

### @docker:CopyFiles{}
- Supported with ballerina services or listeners.

|**Annotation Name**|**Description**|**Default value**|
|--|--|--|
|source|source path of the file (in your machine)|None|
|target|target path (inside container)|None|
|isBallerinaConf|flag whether file is a ballerina config file|false|

### @docker:Expose{}
- Supported with ballerina listeners.

## How to run

1. Download and install JDK 8 or later
2. Get a clone or download the source from this repository (https://github.com/ballerinax/docker)
3. Run the Maven command ``mvn clean  install`` from within the docker directory.
4. Copy ``target/docker-extension-0.9***.jar`` file to ``<BALLERINA_HOME>/bre/lib`` directory.
5. Run ``ballerina build <.bal filename>`` to generate artifacts.

The docker artifacts will be created in a folder called docker with following structure.
```bash
|── docker
|    └── Dockerfile
└── outputfilename.balx
```

### Annotation Usage Sample:
```ballerina
import ballerina/http;
import ballerinax/docker;

@docker:Expose{}
listener http:Listener helloWorldEP = new(9090);

@http:ServiceConfig {
      basePath: "/helloWorld"
}
@docker:Config {
    registry: "docker.abc.com",
    name: "helloworld",
    tag: "v1.0"
}
service helloWorld on helloWorldEP {
    resource function sayHello(http:Caller outboundEP, http:Request request) {
        http:Response response = new;
        response.setTextPayload("Hello, World from service helloWorld ! \n");
        _ = outboundEP -> respond(response);
    }
}
```
**Refer [samples](samples) for more info.**
