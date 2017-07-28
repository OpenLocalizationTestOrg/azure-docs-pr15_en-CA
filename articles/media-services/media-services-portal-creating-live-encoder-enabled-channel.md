<properties 
    pageTitle="How to perform live streaming using Azure Media Services to create multi-bitrate streams with the Azure portal | Microsoft Azure" 
    description="This tutorial walks you through the steps of creating a Channel that receives a single-bitrate live stream and encodes it to multi-bitrate stream using the Azure portal." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>How to perform live streaming using Azure Media Services to create multi-bitrate streams with the Azure portal

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

This tutorial walks you through the steps of creating a **Channel** that receives a single-bitrate live stream and encodes it to multi-bitrate stream.

>[AZURE.NOTE]For more conceptual information related to Channels that are enabled for live encoding, see [Live streaming using Azure Media Services to create multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md).

##<a name="common-live-streaming-scenario"></a>Common Live Streaming Scenario

The following are general steps involved in creating common live streaming applications.

>[AZURE.NOTE] Currently, the max recommended duration of a live event is 8 hours. Please contact  amslived at Microsoft.com if you need to run a Channel for longer periods of time.

1. Connect a video camera to a computer. Launch and configure an on-premises live encoder that can output a single bitrate stream in one of the following protocols: RTMP, Smooth Streaming, or RTP (MPEG-TS). For more information, see [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    This step could also be performed after you create your Channel.

1. Create and start a Channel. 

1. Retrieve the Channel ingest URL. 

    The ingest URL is used by the live encoder to send the stream to the Channel.
1. Retrieve the Channel preview URL. 

    Use this URL to verify that your channel is properly receiving the live stream.

3. Create an event/program (that will also create an asset). 
1. Publish the event (that will create an  OnDemand locator for the associated asset).  

    Make sure to have at least one streaming reserved unit on the streaming endpoint from which you want to stream content.
1. Start the event when you are ready to start streaming and archiving.
2. Optionally, the live encoder can be signaled to start an advertisement. The advertisement is inserted in the output stream.
1. Stop the event whenever you want to stop streaming and archiving the event.
1. Delete the event (and optionally delete the asset).   

##<a name="in-this-tutorial"></a>In this tutorial

In this tutorial, the Azure portal is used to accomplish the following tasks: 

2.  Configure streaming endpoints.
3.  Create a channel that is enabled to perform live encoding.
1.  Get the Ingest URL in order to supply it to live encoder. The live encoder will use this URL to ingest the stream into the Channel. .
1.  Create an event/program (and an asset)
1.  Publish the asset and get streaming URLs  
1.  Play your content 
2.  Cleaning up

##<a name="prerequisites"></a>Prerequisites

The following are required to complete the tutorial.

- To complete this tutorial, you need an Azure account. If you don't have an account, you can create a free trial account in just a couple of minutes. For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).
- A Media Services account. To create a Media Services account, see [Create Account](media-services-portal-create-account.md).
- A webcam and an encoder that can send a single bitrate live stream.

##<a name="configure-streaming-endpoints"></a>Configure streaming endpoints 

Media Services provides dynamic packaging which allows you to deliver your multi-bitrate MP4s in the following streaming formats: MPEG DASH, HLS, Smooth Streaming, or HDS, without you having to re-package into these streaming formats. With dynamic packaging you only need to store and pay for the files in single storage format and Media Services will build and serve the appropriate response based on requests from a client.

To take advantage of dynamic packaging, you need to get at least one streaming unit for the streaming endpoint from which you plan to delivery your content.  

To create and change the number of streaming reserved units, do the following:

1. Log in at the [Azure portal](https://portal.azure.com/) and select your AMS account.
1. In the **Settings** window, click **Streaming endpoints**. 

2. Click on the default streaming endpoint. 

    The **DEFAULT STREAMING ENDPOINT DETAILS** window appears.

3. To specify the number of streaming units, slide the **Streaming units** slider.

    ![Streaming units](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Click the **Save** button to save your changes.

    >[AZURE.NOTE]The allocation of any new units can take up to 20 minutes to complete.

##<a name="create-a-channel"></a>Create a CHANNEL

1. In the [Azure portal](https://portal.azure.com/), select Media Services and then click on your Media Services account name.
2. Select **Live Streaming**.
3. Select **Custom create**. This option will let you create a channel that is enabled for live encoding.

    ![Create a channel](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Click on **Settings**.
    
    1.  Choose the **Live Encoding** channel type. This type specifies that you want to create a Channel that is enabled for live encoding. That means the incoming single bitrate stream is sent to the Channel and encoded into a multi-bitrate stream using specified live encoder settings. For more information, see [Live streaming using Azure Media Services to create multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md). Click OK.
    2. Specify a channel's name.
    3. Click OK at the bottom of the screen.
    
5. Select the **Ingest** tab.

    1. On this page, you can select a streaming protocol. For the **Live Encoding** channel type, valid protocol options are:
        
        - Single bitrate Fragmented MP4 (Smooth Streaming)
        - Single bitrate RTMP
        - RTP (MPEG-TS): MPEG-2 Transport Stream over RTP.
        
        For detailed explanation about each protocol, see [Live streaming using Azure Media Services to create multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md).
    
        You cannot change the protocol option while the Channel or its associated events/programs are running. If you require different protocols, you should create separate channels for each streaming protocol.  

    2. You can apply IP restriction on the ingest. 
    
        You can define the IP addresses that are allowed to ingest a video to this channel. Allowed IP addresses can be specified as either a single IP address (e.g. '10.0.0.1'), an IP range using an IP address and a CIDR subnet mask (e.g. '10.0.0.1/22'), or an IP range using an IP address and a dotted decimal subnet mask (e.g. '10.0.0.1(255.255.252.0)').

        If no IP addresses are specified and there is no rule definition then no IP address will be allowed. To allow any IP address, create a rule and set 0.0.0.0/0.

6. On the **Preview** tab, apply IP restriction on the preview.
7. On the **Encoding** tab, specify the encoding preset. 

    Currently, the only system preset you can select is **Default 720p**. To specify a custom preset, open a Microsoft support ticket. Then, enter the name of the preset created for you. 

>[AZURE.NOTE] Currently, the Channel start can take up to 30 minutes. Channel reset can take up to 5 minutes.

Once you created the Channel, you can click on the channel and select **Settings** where you can view your channels configurations. 

For more information, see [Live streaming using Azure Media Services to create multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md).


##<a name="get-ingest-urls"></a>Get ingest URLs

Once the channel is created, you can get ingest URLs that you will provide to the live encoder. The encoder uses these URLs to input a live stream.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Create and manage events

###<a name="overview"></a>Overview

A channel is associated with events/programs that enable you to control the publishing and storage of segments in a live stream. Channels manage events/programs. The Channel and Program relationship is very similar to traditional media where a channel has a constant stream of content and a program is scoped to some timed event on that channel.

You can specify the number of hours you want to retain the recorded content for the event by setting the **Archive Window** length. This value can be set from a minimum of 5 minutes to a maximum of 25 hours. Archive window length also dictates the maximum amount of time clients can seek back in time from the current live position. Events can run over the specified amount of time, but content that falls behind the window length is continuously discarded. This value of this property also determines how long the client manifests can grow.

Each event is associated with an Asset. To publish the event you must create an OnDemand locator for the associated asset. Having this locator will enable you to build a streaming URL that you can provide to your clients.

A channel supports up to three concurrently running events so you can create multiple archives of the same incoming stream. This allows you to publish and archive different parts of an event as needed. For example, your business requirement is to archive 6 hours of an event, but to broadcast only last 10 minutes. To accomplish this, you need to create two concurrently running event. One event is set to archive 6 hours of the event but the program is not published. The other event is set to archive for 10 minutes and this program is published.

You should not reuse existing programs for new events. Instead, create and start a new program for each event.

Start an event/program when you are ready to start streaming and archiving. Stop the event whenever you want to stop streaming and archiving the event. 

To delete archived content, stop and delete the event and then delete the associated asset. An asset cannot be deleted if it is used by the event; the event must be deleted first. 

Even after you stop and delete the event, the users would be able to stream your archived content as a video on demand, for as long as you do not delete the asset.

If you do want to retain the archived content, but not have it available for streaming, delete the streaming locator.

###<a name="createstartstop-events"></a>Create/start/stop events

Once you have the stream flowing into the Channel you can begin the streaming event by creating an Asset, Program, and Streaming Locator. This will archive the stream and make it available to viewers through the Streaming Endpoint. 

There are two ways to start event: 

1. From the **Channel** page, press **Live Event** to add a new event.

    Specify: event name, asset name, archive window, and encryption option.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    If you left **Publish this live event now** checked, the event the PUBLISHING URLs will get created.
    
    You can press **Start**, whenever you are ready to stream the event.

    Once you start the event, you can press **Watch** to start playing the content.

2. Alternatively, you can use a shortcut and press **Go Live** button on the **Channel** page. This will create a default Asset, Program, and Streaming Locator.

    The event is named **default** and the archive window is set to 8 hours.

You can watch the published event from the **Live event** page. 

If you click **Off Air**, it will stop all live events. 


##<a name="watch-the-event"></a>Watch the event

To watch the event, click **Watch** in the Azure portal or copy the streaming URL and use a player of your choice. 
 
![Created](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Live event automatically converts events to on-demand content when stopped.

##<a name="clean-up"></a>Clean up

If you are done streaming events and want to clean up the resources provisioned earlier, follow the following procedure.

- Stop pushing the stream from the encoder.
- Stop the channel. Once the Channel is stopped, it will not incur any charges. When you need to start it again, it will have the same ingest URL so you won't need to reconfigure your encoder.
- You can stop your Streaming Endpoint, unless you want to continue to provide the archive of your live event as an on-demand stream. If the channel is in stopped state, it will not incur any charges.
  
##<a name="view-archived-content"></a>View archived content

Even after you stop and delete the event, the users would be able to stream your archived content as a video on demand, for as long as you do not delete the asset. An asset cannot be deleted if it is used by an event; the event must be deleted first. 

To manage your assets, select **Setting** and click **Assets**.

![Assets](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Considerations

- Currently, the max recommended duration of a live event is 8 hours. Please contact amslived at Microsoft.com if you need to run a Channel for longer periods of time.
- Make sure to have at least one streaming reserved unit on the streaming endpoint from which you want to stream content.


##<a name="next-step"></a>Next step

Review Media Services learning paths.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Provide feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 