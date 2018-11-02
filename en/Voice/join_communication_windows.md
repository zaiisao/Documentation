
---
title: Join a channel
description: 
platform: Windows
updatedAt: Thu Nov 01 2018 09:04:45 GMT+0000 (UTC)
---
# Join a channel
# Join a Channel
Set the channel profile before the App joins a channel.

## Set the channel profile as communication
After initializing the Agora Engine, call the `setChannelProfile` method to set the channel profile. Agora Engine applies optimization according to the channel profile.

In this methoid, set the channel profile as Communication. This profile applies to voice or video calls such as one-to-one or group calls, where all users in the channel can talk freely. This is the default setting.

> -   Call this method before joining the channel.
> -   One Engine can be specified one profile only. If you want to switch to another profile, destroy the current Engine using `destroy` and create a new one before calling this method to set the new channel profile.

```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_COMMUNICATION);
```

## Join a communication channel
Call the <code>joinChannel</code> method to join a channel. 

In this method:

-   Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Voice/token.md).

-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.

-   Pass a UID that can identify the user. Every user in a channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.


> Once in a channel, a user must call the <code>leaveChannel</code> method to exit the current channel before entering another channel.

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
```
