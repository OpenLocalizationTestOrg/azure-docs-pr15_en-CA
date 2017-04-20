<properties
    pageTitle="Add Push Notifications to Android App with Azure Mobile Apps"
    description="Learn how to use Azure Mobile Apps to send push notifications to your Android app."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Add Push Notifications to your Android App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overview
In this tutorial, you add push notifications to the [Android quick start] project so that a push notification is sent to the device every time a record is inserted.

If you do not use the downloaded quick start server project, you need the push notification extension package. For more information, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Prerequisites

You need the following:

* An IDE depending on your project's backend:

    * [Android Studio](https://developer.android.com/sdk/index.html) if this app has a Node.js backend.

    * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) or later if this app has a .Net backend.

* Android 2.3 or higher, Google Repository revision 27 or higher, and Google Play Services 9.0.2 or higher for Firebase Cloud Messaging.

* Complete the [Android quick start].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Create a project that supports Firebase Cloud Messaging

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Configure a Notification Hub

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Configure Azure to send push notifications

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Enable push notifications for the server project

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Add push notifications to your app

In this section, you update your client Android app to handle push notifications.

### <a name="verify-android-sdk-version"></a>Verify Android SDK Version

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Your next step is to install Google Play services. Google Cloud Messaging has some minimum API level requirements for development and testing, which the **minSdkVersion** property in the Manifest must conform to.

If you are testing with an older device, then consult [Set Up Google Play Services SDK] to determine how low you can set this value, and set it appropriately.

### <a name="add-google-play-services-to-the-project"></a>Add Google Play Services to the project

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Add code

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Test the app against the published mobile service

You can test the app by directly attaching an Android phone with a USB cable, or by using a virtual device in the emulator.

## <a name="more"></a>More

<!-- URLs -->
[Android quick start]: app-service-mobile-android-get-started.md

[Set Up Google Play Services SDK]:https://developers.google.com/android/guides/setup
