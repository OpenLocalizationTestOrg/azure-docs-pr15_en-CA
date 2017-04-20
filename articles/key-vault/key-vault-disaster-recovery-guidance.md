<properties
    pageTitle="What to do in the event of an Azure service disruption that affects Azure Key Vault | Microsoft Azure"
    description="Learn what to do in the event of an Azure service disruption that affects Azure Key Vault."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault availability and redundancy

Azure Key Vault features multiple layers of redundancy to make sure that your keys and secrets remain available to your application even if individual components of the service fail.

The contents of your key vault are replicated within the region and to a secondary region at least 150 miles away but within the same geography. This maintains high durability of your keys and secrets.

If individual components within the key vault service fail, alternate components within the region step in to serve your request to make sure that there is no degradation of functionality. You do not need to take any action to trigger this. It happens automatically and will be transparent to you.

In the rare event that an entire Azure region is unavailable, the requests that you make of Azure Key Vault in that region are automatically routed (*failed over*) to a secondary region. When the primary region is available again, requests are routed back (*failed back*) to the primary region. Again, you do not need to take any action because this happens automatically.

There are a few caveats to be aware of:

* In the event of a region failover, it may take a few minutes for the service to fail over. Requests that are made during this time may fail until the failover completes.
* After a failover is complete, your key vault is in read-only mode. Requests that are supported in this mode are:
 * List key vaults
 * Get properties of key vaults
 * List secrets
 * Get secrets
 * List keys
 * Get (properties of) keys
 * Encrypt
 * Decrypt
 * Wrap
 * Unwrap
 * Verify
 * Sign
 * Backup
* After a failover is failed back, all request types (including read *and* write requests) are available.