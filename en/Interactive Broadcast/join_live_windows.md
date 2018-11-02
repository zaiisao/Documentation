
---
title: Join a Channel
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 04:15:31 GMT+0000 (UTC)
---
# Join a Channel
Set the channel profile before the App joins a channel.

## Set the channel profile as live broadcast
After initializing AgoraRtcEngine, call the <code>setChannelProfile</code> method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In this method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: the host and the audience. The host (broadcaster) sends and receives audio streams while the audience can only receive the audio streams.


> -   Call this method before joining the channel.
> -   One Engine can be specified one profile only. If you want to switch to another profile, destroy the current Engine using <code>release</code> and create a new one before calling this method to set the new channel profile.


```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_LIVE_BROADCASTING);
```


## Join a live broadcast channel
Call the <code>joinChannel</code> method to join a channel. 

In this method:

-   Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Interactive%20Broadcast/token.md).

-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.

-   Pass a UID that can identify the user. Every user in a channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.


> Once in a channel, a user must call the <code>leaveChannel</code> method to exit the current channel before entering another channel.

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
```
