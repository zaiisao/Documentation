
---
title: Join a Channel
description: 
platform: Linux
updatedAt: Fri Nov 09 2018 15:15:26 GMT+0000 (UTC)
---
# Join a Channel
You need to set the channel profile before the app joins a channel.

## Set the channel profile as Live Broadcast
After initializing AgoraRtcEngine, call the <code>setChannelProfile</code> method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In the <code>setChannelProfile</code> method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: Host and Audience. The host (broadcaster) sends and receives audio streams while the audience can only receive the audio streams.


> -   Call the <code>setChannelProfile</code> method before joining a channel.
> -   One engine can be specified one profile only. If you want to switch to another profile, destroy the current engine using the <code>release</code> method and create a new engine before calling the <code>setChannelProfile</code> method to set the new channel profile.


```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_LIVE_BROADCASTING);
```


## Join a live broadcast channel
Call the <code>joinChannel</code> method to join a channel. 

In the <code>joinChannel</code> method:

-   Pass a token that can identify the role and privilege of the user. Set token as null for low security requirements. A token is generated at the server of the application. For how to generate a token, see [Security Keys](../../en/Interactive%20Broadcast/token.md).

-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.

-   Pass a uid that can identify the user. Every user in a channel requires a unique uid. If you want to join the same channel on different devices, ensure that different uids are used for each device.


> Once in a channel, a user must call the <code>leaveChannel</code> method to exit the current channel before entering another channel.

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
```
