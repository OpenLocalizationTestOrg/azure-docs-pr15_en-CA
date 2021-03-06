<properties
   pageTitle="Understanding the OAuth2 implicit grant flow in Azure Active Directory | Microsoft Azure"
   description="Learn more about Azure Active Directory's implementation of the OAuth2 implicit grant flow, and whether it's right for your application."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Understanding the OAuth2 implicit grant flow in Azure Active Directory (AD)

The OAuth2 implicit grant is notorious for being the grant with the longest list of security concerns in the OAuth2 specification. And yet, that is the approach implemented by ADAL JS and the one we recommend when writing SPA applications. What gives? It’s all a matter of tradeoffs: and as it turns out, the implicit grant is the best approach you can pursue for applications that consume a Web API via JavaScript from a browser.

## <a name="what-is-the-oauth2-implicit-grant"></a>What is the OAuth2 implicit grant?

The quintessential [OAuth2 authorization code grant](https://tools.ietf.org/html/rfc6749#section-1.3.1) is the authorization grant which uses two separate endpoints. The authorization endpoint is used for the user interaction phase, which results in an authorization code. The token endpoint is then used by the client for exchanging the code for an access token, and often a refresh token as well. Web applications are required to present their own application credentials to the token endpoint, so that the authorization server can authenticate the client.

The [OAuth2 implicit grant](https://tools.ietf.org/html/rfc6749#section-1.3.2) is a variant allowing a client to obtain an access token (and id_token, in the case of [OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html)) directly from the authorization endpoint, without contacting the token endpoint nor authenticating the client application. This variant was specifically designed for JavaScript based applications running in a Web browser: in the original OAuth2 specification, tokens are returned in a URI fragment. That makes the token bits available to the JavaScript code in the client, but it guarantees they won’t be included in redirects toward the server. Returning tokens via browser redirects directly from the authorization endpoint. It also has the advantage of eliminating any requirements for cross origin calls, which are necessary if the JavaScript application is required to contact the token endpoint.

An important characteristic of the OAuth2 implicit grant is the fact that such flows never return refresh tokens to the client. As we will see in the next section, that isn’t really necessary and would in fact be a security issue.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Suitable scenarios for the OAuth2 implicit grant

As the OAuth2 specification itself declares, the implicit grant has been devised to enable user-agent applications – that is to say, JavaScript applications executing within a browser. The defining characteristic of such applications is that JavaScript code is used for accessing server resources (typically a Web API) and for updating the application UX accordingly. Think of applications like Gmail or Outlook Web Access: when you select a message from your inbox, only the message visualization panel changes to display the new selection, while the rest of the page remains unmodified. This is in contrast with traditional redirect-based Web apps, where every user interaction results in a full page postback and a full page rendering of the new server response.

Applications that take the JavaScript based approach to its extreme are called Single Page Applications, or SPAs: the idea is that those applications only serve an initial HTML page and associated JavaScript, with all subsequent interactions being driven by Web API calls performed via JavaScript. However, hybrid approaches, where the application is mostly postback-driven but performs occasional JS calls, are not uncommon – the discussion about implicit flow usage is relevant for those as well.

Redirect-based applications typically secure their requests via cookies, however, that approach does not work as well for JavaScript applications. Cookies only work against the domain they have been generated for, while JavaScript calls might be directed toward other domains. In fact, that will frequently be the case: think of applications invoking Microsoft Graph API, Office API, Azure API – all residing outside the domain from where the application is served. A growing trend for JavaScript applications is to have no backend at all, relying 100% on 3rd party Web APIs to implement their business function.

Currently, the preferred method of protecting calls to a Web API is to use the OAuth2 bearer token approach, where every call is accompanied by an OAuth2 access token. The Web API examines the incoming access token and, if it finds in it the necessary scopes, it grants access to the requested operation. The implicit flow provides a convenient mechanism for JavaScript applications to obtain access tokens for a Web API, offering numerous advantages in respect to cookies:

- Tokens can be reliably obtained without any need for cross origin calls – mandatory registration of the redirect URI to which tokens are return guarantees that tokens are not displaced
- JavaScript applications can obtain as many access tokens as they need, for as many Web APIs they target – with no restriction on domains
- HTML5 features like session or local storage grant full control over token caching and lifetime management, whereas cookies management is opaque to the app
- Access tokens aren’t susceptible to Cross-site request forgery (CSRF) attacks

The implicit grant flow does not issue refresh tokens, mostly for security reasons. A refresh token isn’t as narrowly scoped as access tokens, granting far more power hence inflicting far more damage in case it is leaked out. In the implicit flow, tokens are delivered in the URL, hence the risk of interception is higher than in the authorization code grant.

However, note that a JavaScript application has another mechanism at its disposal for renewing access tokens without repeatedly prompting the user for credentials. The application can use a hidden iframe to perform new token requests against the authorization endpoint of Azure AD: as long as the browser still has an active session (read: has a session cookie) against the Azure AD domain, the authentication request can successfully occur without any need for user interaction. 

This model grants the JavaScript application the ability to independently renew access tokens and even acquire new ones for a new API (provided that the user previously consented for them. This avoids the added burden of acquiring, maintaining and protecting a high value artifact such as a refresh token. The artifact which makes the silent renewal possible, the Azure AD session cookie, is managed outside of the application. Another advantage of this approach is a user can sign out from Azure AD, using any of the applications signed into Azure AD, running in any of the browser tabs. This results in the deletion of the Azure AD session cookie, and the JavaScript application will automatically lose the ability to renew tokens for the signed out user.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Is the implicit grant suitable for my app?

The implicit grant presents more risks than other grants. The areas you need to pay attention to are well documented (see for example [Misuse of Access Token to Impersonate Resource Owner in Implicit Flow][OAuth2-Spec-Implicit-Misuse] and [OAuth 2.0 Threat Model and Security Considerations][OAuth2-Threat-Model-And-Security-Implications]). However, the higher risk profile is largely due to the fact that it is meant to enable applications that execute active code, served by a remote resource to a browser. If you are planning an SPA architecture, have no backend components or intend to invoke a Web API via JavaScript, use of the implicit flow for token acquisition is recommended.

If your application is a native client, the implicit flow isn’t a great fit. The absence of the Azure AD session cookie in the context of a native client deprives your application from the means of maintaining a long lived session. Which means your application will repeatedly prompt the user when obtaining access tokens for new resources.

If you are developing a Web application which includes a backend, and consuming an API from its backend code, the implicit flow is also not a good fit. Other grants give you far more power. For example, the OAuth2 client credentials grant provides the ability to obtain tokens that reflect the permissions assigned to the application itself, as opposed to user delegations. This means the client has the ability to maintain programmatic access to resources even when a user is not actively engaged in a session, and so on. Not only that, but such grants give higher security guarantees. For instance, access tokens never transit through the user browser, they don’t risk being saved in the browser history, and so on. The client application can also perform strong authentication when requesting a token.

## <a name="next-steps"></a>Next steps

- For a complete list of developer resources, including reference information for the protocols and OAuth2 authorization grant flows support by Azure AD, refer to the [Azure AD Developer's Guide][AAD-Developers-Guide]
- See [How to integrate an application with Azure AD] [ACOM-How-To-Integrate] for additional depth on the application integration process.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

