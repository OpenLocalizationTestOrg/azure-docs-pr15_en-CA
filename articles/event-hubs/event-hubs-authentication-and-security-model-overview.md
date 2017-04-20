<properties 
    pageTitle="Overview of Event Hubs authentication and security model | Microsoft Azure"
    description="Event Hubs authentication and security model overview."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Event Hubs authentication and security model overview

The Event Hubs security model meets the following requirements:

- Only devices that present valid credentials can send data to an Event Hub.
- A device cannot impersonate another device.
- A rogue device can be blocked from sending data to an Event Hub.

## <a name="device-authentication"></a>Device authentication

The Event Hubs security model is based on a combination of [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) tokens and *event publishers*. An event publisher defines a virtual endpoint for an Event Hub. The publisher can only be used to send messages to an Event Hub. It is not possible to receive messages from a publisher.

Typically, an Event Hub employs one publisher per device. All messages that are sent to any of the publishers of an Event Hub are enqueued within that Event Hub. Publishers enable fine-grained access control and throttling.

Each device is assigned a unique token, which is uploaded to the device. The tokens are produced such that each unique token grants access to a different unique publisher. A device that possesses a token can only send to one publisher, but no other publisher. If multiple devices share the same token, then each of these devices shares a publisher.

Although not recommended, it is possible to equip devices with tokens that grant direct access to an Event Hub. Any device that holds this token can send messages directly into that Event Hub. Such a device will not be subject to throttling. Furthermore, the device cannot be blacklisted from sending to that Event Hub.

All tokens are signed with a SAS key. Typically, all tokens are signed with the same key. Devices are not aware of the key; this prevents devices from manufacturing tokens.

### <a name="create-the-sas-key"></a>Create the SAS key

When creating an Event Hubs namespace, Azure Event Hubs generates a 256-bit SAS key named **RootManageSharedAccessKey**. This key grants send, listen, and manage rights to the namespace. You can create additional keys. It is recommended that you produce a key that grants send permissions to the specific Event Hub. For the remainder of this topic, it is assumed that you named this key `EventHubSendKey`.

The following example creates a send-only key when creating the Event Hub:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Generate tokens

You can generate tokens using the SAS key. You must produce only one token per device. Tokens can then be produced using the following method. All tokens are generated using the **EventHubSendKey** key. Each token is assigned a unique URI.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

When calling this method, the URI should be specified as `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. For all tokens, the URI is identical, with the exception of `PUBLISHER_NAME`, which should be different for each token. Ideally, `PUBLISHER_NAME` represents the ID of the device that receives that token.

This method generates a token with the following structure:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

The token expiration time is specified in seconds from Jan 1, 1970. The following is an example of a token:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Typically, the tokens have a lifespan that resembles or exceeds the lifespan of the device. If the device has the capability to obtain a new token, tokens with a shorter lifespan can be used.

### <a name="devices-sending-data"></a>Devices sending data

Once the tokens have been created, each device is provisioned with its own unique token.

When the device sends data into an Event Hub, the device tags its token with the send request. To prevent an attacker from eavesdropping and stealing the token, the communication between the device and the Event Hub must occur over an encrypted channel.

### <a name="blacklisting-devices"></a>Blacklisting devices

If a token is stolen by an attacker, the attacker can impersonate the device whose token has been stolen. Blacklisting a device renders the device unusable until the device is given a new token that uses a different publisher.

## <a name="authentication-of-back-end-applications"></a>Authentication of back-end applications

To authenticate back-end applications that consume the data generated by devices, Event Hubs employs a security model that is similar to the model that is used for Service Bus topics. An Event Hubs consumer group is equivalent to a subscription to a Service Bus topic. A client can create a consumer group if the request to create the consumer group is accompanied by a token that grants manage privileges for the Event Hub, or for the namespace to which the Event Hub belongs. A client is allowed to consume data from a consumer group if the receive request is accompanied by a token that grants receive rights on that consumer group, the Event Hub, or the namespace to which the Event Hub belongs.

The current version of Service Bus does not support SAS rules for individual subscriptions. The same holds true for Event Hubs consumer groups. SAS support will be added for both features in the future.

In the absence of SAS authentication for individual consumer groups, you can use SAS keys to secure all consumer groups with a common key. This approach enables an application to consume data from any of the consumer groups of an Event Hub.

## <a name="next-steps"></a>Next steps

To learn more about Event Hubs, visit the following topics:

- [Event Hubs overview]
- A [queued messaging solution] using Service Bus queues.
- A complete [sample application that uses Event Hubs].

[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 