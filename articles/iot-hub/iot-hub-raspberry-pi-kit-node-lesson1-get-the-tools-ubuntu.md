<properties
 pageTitle="Get the tools (Ubuntu 16.04) | Microsoft Azure"
 description="Download and install the necessary tools and software for the first sample application for your Pi on Ubuntu."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="12-get-the-tools-ubuntu-1604"></a>1.2 Get the tools (Ubuntu 16.04)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 What you will do

Download the development tools and the software for the first sample application for your Raspberry Pi 3. If you meet any problems, seek solutions in the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 What you will learn

In this section, you will learn:

- How to install Git and Node.js
  - [Git](https://git-scm.com) is an open source distributed version control system. The sample application for this lesson is stored on Git.
  - [Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.
- How to use NPM to install additional Node.js development tools.
  - The minimum required version of Node.js is 4.5 LTS.
  - [NPM](https://www.npmjs.com) is one of the package managers for Node.js

## <a name="123-what-do-you-need"></a>1.2.3 What do you need

- An Internet connection to download the development tools and the software
- A computer that is running Ubuntu 16.04 or later 

## <a name="124-install-git-nodejs-and-npm"></a>1.2.4 Install Git, Node.js, and NPM

Use the keyboard shortcut `Ctrl + Alt + T` to open a Terminal window and run the following commands:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 Install additional Node.js development tools

You use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to your Pi. You also use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.

Install `gulp` and `device-discovery-cli` by running the following command in the Terminal window:

```bash
sudo npm install -g device-discovery-cli gulp
```

If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.

## <a name="126-install-visual-studio-code"></a>1.2.6 Install Visual Studio Code

[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code. Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS. You use this editor later in the tutorial to edit the sample code.

## <a name="127-summary"></a>1.2.7 Summary

You've installed the required development tools and software for the first sample application. In the next section, you create, deploy, and run the sample application on your Pi.

## <a name="next-steps"></a>Next Steps

[1.3 Create and deploy the blink sample application](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)