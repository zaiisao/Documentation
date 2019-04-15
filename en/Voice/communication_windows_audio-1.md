
---
title: Making a Voice Call
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 04:07:08 GMT+0800 (CST)
---
# Making a Voice Call
In this quickstart, you will learn how to use the Agora SDK to make a voice call.

## Prerequisites

1.  See [Setting up Your Development Environment](../../en/Quickstart%20Guide/windows_video.md) for the prerequisites, environment setup, and SDK integration guide.

2.  See [Agora Windows Video Call Tutorial Sample App](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-Windows) to better understand how a Sample App is created from the scratch.


## Quickstart

### Create AgoraRtcEngine Instance and Initialize

Create an AgoraRtcEngine instance by invoking <code>create</code>, and initialize the instance by invoking <code>initialize</code>.

Pass the [Agora App ID](../../en/Agora%20Platform/token.md). Only applications with the same App ID can join the same channel.

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

### Joining a Channel

Now, you can join a voice call channel. Call the <code>joinChannel</code> method to join a channel. Users in the same channel can talk to each other, and multiple users in the same channel can initiate a group chat.

In this method:

-   Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Agora%20Platform/token.md).

-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.

-   Pass a UID that can identify the user. Every user in the channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.


> Once in a call, a user must call the <code>leaveChannel</code> method to exit the current call before entering into another channel.

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
```

### Ending a Call

When a voice call ends, call the <code>leaveChannel</code> method to leave the channel.

This method releases all resources related to the call. When the user leaves the channel, the SDK triggers the <code>onleaveChannel</code> callback.

```
int nRet = m_lpAgoraEngine->leaveChannel();
```

> If the <code>release</code> method is called immediately after <code>leaveChannel</code>, the process will be interrupted, and the SDK will not trigger the <code>onleaveChannel</code> callback.

## Conclusion

You can now start a one-to-one or group voice call. The only difference between a one-to-one and group call is the number of users in the channel.

## Reference

The following list are references to the Agora APIs used on this page in making a voice call:

-   Create an agora::IRtcEngine Object: <code>create</code>.

-   Initialize the agora::IRtcEngine object:  <code>initialize</code>.

-   Join a channel: <code>joinChannel</code>

-   Leave the channel:  <code>leaveChannel</code>

-   Release the IRtcEngine object: <code>release</code>


You can find all the Agora APIs that can be implemented in a voice call in the [Voice Call API](https://docs.agora.io/en/Voice/API%20Reference/cpp/index.html).


