
---
title: Cocos Creator Quickstart
description: 
platform: Cocos Creator
updatedAt: Tue Oct 22 2019 06:19:15 GMT+0800 (CST)
---
# Cocos Creator Quickstart
## Prerequisites

Development environment:

- Cocos Creator v2.0.9 or later.
- A certified Cocos Creator account.
- An Android or iOS device for the mobile end.
- No VPN connection.
- Ensure that the browser for the Web end meets the [requirements](https://docs.agora.io/en/Audio%20Broadcast/web_prepare?platform=Web).

## Integrate the Agora Service

Create a Cocos Creator project and enable the Agora Service in your project with the following steps:

1. Open your project with Cocos Creator. Choose **Panel** and **Service** in the drop-down menu. The **Service** panel appears on the right.

2. On the **Service** panel, select the game your just created and click **Association**.

	 ![](https://web-cdn.agora.io/docs-files/1562229233008)

	When the association completes, the **Agora Service** panel appears in the **Service** panel.
	 
   ![](https://web-cdn.agora.io/docs-files/1562229358413)

3. Enable the **Agora Service** panel, and click **OK** in the pop-up window.

   ![](https://web-cdn.agora.io/docs-files/1562297897751)
	 
4. Follow the on-screen instructions and click **open service**.

   ![](https://web-cdn.agora.io/docs-files/1562229434363)
	 
5. Upon opening the service, the following window appears. Click **console** to go to Agora **Console**. 

	![](https://web-cdn.agora.io/docs-files/1562229467151)

	The Console automatically creates an Agora project. Click open the project and get your Agora App ID. You need to pass the App ID when initializing the RtcEngine.

  ![](https://web-cdn.agora.io/docs-files/1562297985211)
	 
   Now you successfully enable the **Agora Service** for your Cocos Creator project and get the Agora App ID. Cocos Creator automatically downloads and configures all the Agora's depenencies.

## Call the APIs to enable the audio function

You can now call the APIs to enable the audio function in your Cocos Creator project:

![](https://web-cdn.agora.io/docs-files/1562322588460)

### API call sequence

Enable the audio function with the following APIs:

1. Initialize the RtcEngine: [agora undefined agora.joinChannel("", channel, "", 0);](../../en/Interactive%20Gaming/game_coco.md)
3. Leave the channel:  [agora && agora.leaveChannel();](../../en/Interactive%20Gaming/game_coco.md)

Register the following callback events in the `agora.on` method:

* `agora.on('join-channel-success', this.onJoinChannelSuccess, this);`: Occurs when the user joins the channel.
* `agora.on('leave-channel', this.onLeaveChannel, this);`: Occurs when the user leaves the channel.
* `agora.on('rejoin-channel-success', this.onRejoinChannelSuccess, this);`: Occurs when the user rejoins the channel.

For more callback events, see the API reference for [agora.on](../../en/Interactive%20Gaming/game_coco.md).

### Sample Code

Agora provides an open-souce [Cocos Creator Voice Demo](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-CocosCreator-Voice-Agora). You can download and refer to the code logic in the sample code.

### API reference
	 
The [JavaScript APIs](../../en/Interactive%20Gaming/game_coco.md) provided by Cocos Creator only covers major functions. For more advanced functions, see the full API reference documents on other platforms.
