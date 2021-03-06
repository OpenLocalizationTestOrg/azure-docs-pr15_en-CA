<properties 
    pageTitle="Get Started with Azure Active Directory and Visual Studio connected services (WebApi projects) | Microsoft Azure" 
    description="How to get started using Azure Active Directory in WebApi projects after connecting to or creating an Azure AD using Visual Studio connected services" 
  services="active-directory"
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Get Started with Azure Active Directory and Visual Studio connected services (WebApi projects)

> [AZURE.SELECTOR]
> - [Getting Started](vs-active-directory-webapi-getting-started.md)
> - [What Happened](vs-active-directory-webapi-what-happened.md)

##<a name="requiring-authentication-to-access-controllers"></a>Requiring authentication to access controllers
 
All controllers in your project were adorned with the **Authorize** attribute. This attribute will require the user to be authenticated before accessing the APIs defined by these controllers. To allow the controller to be accessed anonymously, remove this attribute from the controller. If you want to set the permissions at a more granular level, apply the attribute to each method that requires authorization instead of applying it to the controller class.

[Learn more about Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
 