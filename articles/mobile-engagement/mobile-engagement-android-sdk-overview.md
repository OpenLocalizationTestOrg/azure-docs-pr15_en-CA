<properties
    pageTitle="Android SDK Integration for Azure Mobile Engagement"
    description="Describes how to integrate Azure Mobile Engagement SDK in Android apps"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Android SDK Integration for Azure Mobile Engagement

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS](mobile-engagement-ios-sdk-overview.md)
- [Android](mobile-engagement-android-sdk-overview.md)

This document describes all the integration and configuration options available for Azure Mobile Engagement Android SDK.

## <a name="prerequisites"></a>Prerequisites

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Advanced Features

### <a name="reporting-features"></a>Reporting Features

You can add these features:

1. [Advanced reporting options](mobile-engagement-android-advanced-reporting.md)
2. [Location Reporting options](mobile-engagement-android-location-reporting.md)
3. [Advanced Configuration options](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Notifications:
[How to integrate Reach (Notifications) in your Android app](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM): [How to Integrate GCM with Mobile Engagement](mobile-engagement-android-gcm-integrate.md)

2. Amazon Device Messaging (ADM): [How to Integrate ADM with Mobile Engagement](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Tag plan implementation:
[How to use the advanced Mobile Engagement tagging API in your Android app](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Release notes

### <a name="423-08102016"></a>4.2.3 (08/10/2016)

 - No more WIFI lock.
 - Fix a deadlock when calling getDeviceId before init (bug introduced in 4.2.0).

For all versions, see the [complete release notes](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Upgrade procedures

If you already have integrated an older version of our SDK into your application, consult [Upgrade Procedures](mobile-engagement-android-upgrade-procedure.md).
