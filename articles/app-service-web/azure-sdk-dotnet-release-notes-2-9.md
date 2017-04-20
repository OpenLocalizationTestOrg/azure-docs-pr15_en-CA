<properties 
   pageTitle="Azure SDK for .NET 2.9 Release Notes" 
   description="Azure SDK for .NET 2.9 Release Notes" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-29-release-notes"></a>Azure SDK for .NET 2.9 Release Notes

##<a name="overview"></a>Overview

This document contains the release notes for the Azure SDK for .NET 2.9 release. 

For detailed information about updates in this release, see the [Azure SDK 2.9 announcement post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure SDK 2.9 for Visual Studio 2015 Update 2 and Visual Studio "15" Preview
 
This update includes the following bug fixes:

- Issue related to REST API Client Generation in in which the string "Unknown Type” would appear as the name of the code-gen folder and/or the name of the namespace dropped into the generated code.
- Issue related to Scheduled WebJobs in which the authentication information was failing to be passed to the Scheduler provisioning process.

This update includes the following new feature:

- Support for secondary App Services in the "Services" tab of the App Service provisioning dialog. 

##<a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure Data Lake Tools for Visual Studio 2015 Update 2
 
This updates includes the following:

- **Azure Data Lake Tools** for Visual Studio is now merged into the Azure SDK for .NET release. The tool is automatically installed when you install Azure SDK. 

    The tool is updated frequently, go [here](http://aka.ms/datalaketool) to get the updates.

- **Server Explorer** now enables you to view all and create some U-SQL metadata entities. For more information, see [this](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.


##<a name="hdinsight-tools"></a>HDInsight Tools 

**HDInsight Tools** for Visual Studio now supports HDInsight version 3.3, including showing Tez graphs and other language fixes.


##<a name="azure-resource-manager"></a>Azure Resource Manager 

This release adds [KeyVault](../resource-manager-keyvault-parameter.md) support for ARM templates.

##<a name="see-also"></a>See also

[Azure SDK 2.9 announcement post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)
