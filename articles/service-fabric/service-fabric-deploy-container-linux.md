<properties
   pageTitle="Service Fabric and Deploying Containers in Linux | Microsoft Azure"
   description="Service Fabric and the use of Docker containers to deploy microservice applications. This article describes the capabilities that Service Fabric provides for containers and how to deploy a Docker container image into a cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Deploy a Docker container to Service Fabric

> [AZURE.SELECTOR]
- [Deploy Windows Container](service-fabric-deploy-container.md)
- [Deploy Docker Container](service-fabric-deploy-container-linux.md)

This article walks you through building containerized services in Docker containers on Linux.

Service Fabric has several container capabilities that help you with building applications that are composed of microservices that are containerized. These are called containerized services.

The capabilities include;

- Container image deployment and activation
- Resource governance
- Repository authentication
- Container port to host port mapping
- Container-to-container discovery and communication
- Ability to configure and set environment variables


## <a name="packaging-a-docker-container-with-yeoman"></a>Packaging a docker container with yeoman
When packaging a container on Linux, you can choose either to use a yeoman template or [create the application package manually](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

A Service Fabric application can contain one or more containers, each with a specific role in delivering the application's functionality. The Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy to create your application and add a container image. Let's use Yeoman to create a new application with a single Docker container called *SimpleContainerApp*. You can add more services later by editing the generated manifest files.

## <a name="create-the-application"></a>Create the application

1. In a terminal, type **yo azuresfguest**.

2. For the framework choose **Container** .

3. Name your application e.g. SimpleContainerApp

4. Provide the URL for the container image from a DockerHub repo. This takes the form [repo]/[image name]

![Service Fabric Yeoman generator for containers][sf-yeoman]

## <a name="deploy-the-application"></a>Deploy the application

Once the application is built, you can deploy it to the local cluster using the Azure CLI.

1. Connect to the local Service Fabric cluster.

    ```bash
    azure servicefabric cluster connect
    ```

2. Use the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.

    ```bash
    ./install.sh
    ```

3. Open a browser and navigate to Service Fabric Explorer at http://localhost:19080/Explorer (replace localhost with the private IP of the VM if using Vagrant on Mac OS X).

4. Expand the Applications node and note that there is now an entry for your application type and another for the first instance of that type.

5. Use the uninstall script provided in the template to delete the application instance and unregister the application type.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Next steps

- [Overview of Service Fabric and containers](service-fabric-containers-overview.md)
- [Interacting with Service Fabric clusters using the Azure CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

