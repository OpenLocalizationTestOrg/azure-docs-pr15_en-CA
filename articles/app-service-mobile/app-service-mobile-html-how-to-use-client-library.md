<properties
    pageTitle="How to Use the JavaScript SDK for Azure Mobile Apps"
    description="How to Use v for Azure Mobile Apps"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>How to Use the JavaScript Client Library for Azure Mobile Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

This guide teaches you to perform common scenarios using the latest [JavaScript SDK for Azure Mobile Apps]. If you are new to Azure Mobile Apps, first complete [Azure Mobile Apps Quick Start] to create a backend and create a table. In this guide, we focus on using the mobile backend in HTML/JavaScript Web applications.

## <a name="supported-platforms"></a>Supported Platforms

We limit browser support to the current and last versions of the major browsers:  Google Chrome, Microsoft Edge, Microsoft Internet Explorer, and Mozilla Firefox.  We expect the SDK to function with any relatively modern browser.

The package is distributed as a Universal JavaScript Module, so it supports globals, AMD, and CommonJS formats.

##<a name="Setup"></a>Setup and Prerequisites

This guide assumes that you have created a backend with a table. This guide assumes that the table has the same schema as the tables in those tutorials.

Installing the Azure Mobile Apps JavaScript SDK can be done via the `npm` command:

```
npm install azure-mobile-apps-client --save
```

Once installed, the library is located in `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Copy this file to your web area.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

The library can also be used as an ES2015 module, within CommonJS environments such as Browserify and Webpack and as an AMD library.  For example:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>How to: Authenticate Users

Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter. You can set permissions on tables to restrict access for specific operations to only authenticated users. You can also use the identity of authenticated users to implement authorization rules in server scripts. For more information, see the [Get started with authentication] tutorial.

Two authentication flows are supported: a server flow and a client flow.  The server flow provides the simplest authentication experience, as it relies on the provider's web authentication interface. The client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific SDKs.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for External Redirect URLs.

Several types of JavaScript applications use a loopback capability to handle OAuth UI flows.  These capabilities include:

* Running your service locally
* Using Live Reload with the Ionic Framework
* Redirecting to App Service for authentication. 

Running locally can cause problems because, by default, App Service authentication is only configured to allow access from your Mobile App backend. Use the following steps to change the App Service settings to enable authentication when running the server locally:

1. Log in to the [Azure portal]
2. Navigate to your Mobile App backend.
3. Select **Resource explorer** in the **DEVELOPMENT TOOLS** menu.
4. Click **Go** to open the resource explorer for your Mobile App backend in a new tab or window.
5. Expand the **config** > **authsettings** node for your app.
6. Click the **Edit** button to enable editing of the resource.
7. Find the **allowedExternalRedirectUrls** element, which should be null. Add your URLs in an array:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Replace the URLs in the array with the URLs of your service, which in this example is `http://localhost:3000` for the local Node.js sample service. You could also use `http://localhost:4400` for the Ripple service or some other URL, depending on how your app is configured.

8. At the top of the page, click **Read/Write**, then click **PUT** to save your updates.

You also need to add the same loopback URLs to the CORS whitelist settings:

1. Navigate back to the [Azure portal].
2. Navigate to your Mobile App backend.
3. Click **CORS** in the **API** menu.
4. Enter each URL in the empty **Allowed Origins** text box.  A new text box is created.
5. Click **SAVE**
    
After the backend updates, you will be able to use the new loopback URLs in your app.

<!-- URLs. -->
[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md
[Get started with authentication]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portal]: https://portal.azure.com/
[JavaScript SDK for Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

