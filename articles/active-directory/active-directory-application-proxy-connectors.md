<properties
    pageTitle="Working with Azure AD Application Proxy connectors | Microsoft Azure"
    description="Covers how to create and manage groups of connectors in Azure AD Application Proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publish applications on separate networks and locations using connector groups

> [AZURE.SELECTOR]
- [Azure portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure classic portal](active-directory-application-proxy-connectors.md)


Connector groups are useful for various scenarios, including:

- Sites with multiple interconnected datacenters. In this case, you want to keep as much traffic within the datacenter as possible because cross-datacenter links are usually expensive and slow. You can deploy connectors in each datacenter to serve only the applications that reside within the datacenter. This approach minimizes cross-datacenter links and provides an entirely transparent experience to your users.
- Managing applications installed on isolated networks that are not part of the main corporate network. You can use connector groups to install dedicated connectors on isolated networks to also isolate applications to the network.
- For applications installed on IaaS for cloud access, connector groups provide a common service to secure the access to all the apps. Connector groups don't create additional dependency on your corporate network, or fragment the app experience. Connectors can be installed on every cloud datacenter and serve only applications that reside in this network. You can install several connectors to achieve high availability.
- Support for multi-forest environments in which specific connectors can be deployed per forest and set to serve specific applications.
- Connector groups can be used in Disaster Recovery sites to either detect failover or as backup for the main site.
- Connector groups can also be used to serve multiple companies from a single tenant.

## <a name="prerequisite-create-your-connectors"></a>Prerequisite: Create your connectors
In order to group your connectors, you have to make sure you [installed multiple connectors](active-directory-application-proxy-enable.md), and that you name them and then group them. Finally you have to assign them to specific apps.

## <a name="step-1-create-connector-groups"></a>Step 1: Create connector groups
You can create as many connector groups as you want. Connector group creation is accomplished in the Azure classic portal.

1. Select your directory and click **Configure**.  
    ![Application proxy, configure screenshot - click manage connector groups](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. Under Application Proxy, click **Manage Connector Groups** and create a new connector group by giving the group a name.  
    ![Application proxy connector groups screenshot - name new group](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Step 2: Assign connectors to your groups
Once the connector groups are created, move the connectors to the appropriate group.

1. Under **Application Proxy**, click **Manage Connectors**.
2. Under **Group**, select the group you want for each connector. It might take the connectors up to 10 minutes to become active in the new group.  
    ![Application proxy connectors screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Step 3: Assign applications to your connector groups
The last step is to set each application to the connector group that will serve it.

1. In the Azure classic portal, in your directory, select the Application you want to assign to the group and click **Configure**.
2. Under **Connector group**, select the group you want the application to use. This change is immediately applied.  
    ![Application proxy connector group screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>See also

- [Enable Application Proxy](active-directory-application-proxy-enable.md)
- [Enable single-sign on](active-directory-application-proxy-sso-using-kcd.md)
- [Enable conditional access](active-directory-application-proxy-conditional-access.md)
- [Troubleshoot issues you're having with Application Proxy](active-directory-application-proxy-troubleshoot.md)

For the latest news and updates, check out the [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)
