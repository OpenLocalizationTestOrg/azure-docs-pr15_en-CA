<properties 
    pageTitle="Windows Universal Apps SDK Release Notes" 
    description="Azure Mobile Engagement - Windows Universal Apps SDK Release Notes"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-universal-apps-sdk-release-notes"></a>Windows Universal Apps SDK Release Notes

##<a name="340-04192016"></a>3.4.0 (04/19/2016)

-   Reach overlay improvements.
-   Added "TestLogLevel" API to enable/disable/filter console logs emitted by the SDK.
-   Fixed in-activity notifications targeting the first activity not displayed on App start.

##<a name="331-02182016"></a>3.3.1 (02/18/2016)

-   Fixed conflicts between web announcement's HTML content and SDK's HTML page.
-   Stability improvements.

##<a name="330-01222016"></a>3.3.0 (01/22/2016)

-   Fixed crash formatting on UWP apps running in release mode.
-   Fixed 1px margin on notifications for Universal 8.1 apps.
-   ms-appx and ms-appdata schemes available on action urls.
-   Stability improvements.

##<a name="320-11202015"></a>3.2.0 (11/20/2015)

-   Added support for Windows 10 Universal Windows Platform applications.
-   Added push channel sharing feature to fix channel conflicts (now compatible with Azure Notification Hubs).
-   Fixed crash while requesting the device id just after the initialization.
-   Console logs improvements.
-   Fixed crash while parsing some unhandled exceptions.

##<a name="310-05212015"></a>3.1.0 (05/21/2015)

-   The Mobile Engagement device id is now based on a GUID generated at installation time.

##<a name="301-04292015"></a>3.0.1 (04/29/2015)

-   Fixed a bug affecting the SDK initialization on some Windows Phone WinRT apps.

##<a name="300-04032015"></a>3.0.0 (04/03/2015)

-   Introducing the Mobile Engagement SDK for Universal App (Windows and Windows Phone WinRT).
-   Default notification icon updated.
-   Send back system notification action feedback when a notification is clicked.
-   Fixed system notification which is sometimes replayed in-app after being clicked.

##<a name="200-02172015"></a>2.0.0 (02/17/2015)

-   Initial Release of Azure Mobile Engagement
-   appId/sdkKey configuration is replaced by a connection string configuration.
-   Security improvements.

 
