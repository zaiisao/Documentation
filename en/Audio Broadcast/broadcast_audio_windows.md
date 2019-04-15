
---
title: Starting a Live Voice Broadcast
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 04:09:46 GMT+0800 (CST)
---
# Starting a Live Voice Broadcast
In this quickstart, you will learn how to use the Agora SDK to start a live audio broadcast.

## Prerequisites

1.  See [Setting up Your Development Environment](../../en/Quickstart%20Guide/windows_video.md) for the prerequisites, environment setup, and SDK integration guide.

2.  See [Agora Windows Video Live Tutorial Sample App](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-Windows) to better understand how a Sample App is created from the scratch.


## Quickstart

### Create AgoraRtcEngine Instance and Initialize

Create an AgoraRtcEngine instance by invoking <code>create</code>, and initialize the instance by invoking <code>initialize</code>.

Pass the [Agora App ID](../../en/Quickstart%20Guide/windows_video.md). Only applications with the same App ID can join the same channel.

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

### Setting the Channel Profile

After initializing AgoraRtcEngine, call the <code>setChannelProfile</code> method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In this method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: the host and the audience. The host (broadcaster) sends and receives audio streams while the audience can only receive the audio streams.


> -   Call this method before joining the channel.
> -   One Engine can be specified one profile only. If you want to switch to another profile, destroy the current Engine using <code>release</code> and create a new one before calling this method to set the new channel profile.


```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_LIVE_BROADCASTING);
```

### Setting the User Role

An interactive broadcast channel consists of two user roles: the host and the audience. Call the <code>setClientRole</code> method to set the user role as broadcaster or audience according to your needs. The differences of the two roles are:

-   Host (Broadcaster): A host receives and publishes the audio streams. The host can also interact with other hosts using voice.

-   Audience: An audience receives the audio streams of the host(s).


```
int nRet = m_lpAgoraEngine->setClientRole(role);
```

### Joining a Channel

Now, you can join a live broadcast channel. Call the <code>joinChannel</code> method to join a channel. Hosts in the same channel can talk to and see each other, and audiences in the same channel can hear the hosts.

In this method:

-   Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Agora%20Platform/token.md).

-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.

-   Pass a UID that can identify the user. Every user in a channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.


> Once in a channel, a user must call the <code>leaveChannel</code> method to exit the current channel before entering another channel.

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
```

### Ending a Live Broadcast

When a live broadcast ends, call the <code>leaveChannel</code> method to leave the channel.

This method releases all resources related to the live broadcast. When the user leaves the channel, the SDK triggers the <code>onleaveChannel</code> callback.

```
int nRet = m_lpAgoraEngine->leaveChannel();
```


> If the <code>leaveChannel</code> method is called immediately after <code>release</code>, the process will be interrupted, and the SDK will not trigger the <code>onleaveChannel</code> callback.

## Conclusion

You can now start a audio live broadcast.

## Reference

Followings are part of the Agora APIs that we use to start a live broadcast:

-   Create an agora::IRtcEngine object: <code>create</code>

-   Initialize the IRtcEngine obejct: <code>initialize</code>

-   Set a Channel Profile:  <code>setChannelProfile</code>

-   Release the IRtcEngine Object:  <code>release</code>

-   Set the User Role:  <code>setClientRole</code>

-   Join a Channel:  <code>joinChannel</code>

-   Leave a Channel:  <code>leaveChannel</code>


You can find all the Agora APIs that can be implemented in a live broadcast in [Interactive Broadcast API](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/index.html).



