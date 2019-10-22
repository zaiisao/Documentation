
---
title: Join a Channel
description: 
platform: Windows
updatedAt: Fri Jul 19 2019 09:22:57 GMT+0800 (CST)
---
# Join a Channel
Before joining the channel, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/windows_video.md).

## Implementation

You need to set the channel profile before the app joins a channel.

### Set the channel profile as Live Broadcast
After initializing AgoraRtcEngine, call the <code>setChannelProfile</code> method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In the <code>setChannelProfile</code> method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles:

- The host (broadcaster) sends and receives audio streams.
- The audience receives audio streams.


> -   Call the <code>setChannelProfile</code> method before joining a channel.
> -   One engine can be specified one profile only. If you want to switch to another profile, destroy the current engine using the <code>release</code> method and create a new engine before calling the <code>setChannelProfile</code> method to set the new channel profile.


```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_LIVE_BROADCASTING);
```


### Join a live broadcast channel
Call the <code>joinChannel</code> method to join a channel. 

In the <code>joinChannel</code> method:

-  Pass a token that identifies the role and privilege of the user. 
	- For the testing environment, we recommend usign a Temp Token generated on Console. See [Get a Temp Token](../../en/Interactive%20Broadcast/token.md).
	- For the production environment, we recommend using a Token generated at your server. For how to generate a token, see [Token Security](../../en/Interactive%20Broadcast/token_server.md). 
-   Pass a channel ID that identifies the channel. Users that input the same channel ID enter into the same channel.
-   Pass a uid that identifies the user. Each user in a channel requires a unique uid. If you want to join the same channel on different devices, ensure that different uids are used for each device.


> Once in a channel, a user must call the <code>leaveChannel</code> method to exit the current channel before entering another channel.

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpToken, lpChannelName, lpStreamInfo, nUID);
```

## Next Steps
You are in the channel and can start a live broadcast with the following step:

- [Switch the Client Role](../../en/Interactive%20Broadcast/role_windows.md)
- [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_windows_live.md)

For other functions such as manipulating the audio volume, audio effect, or video resolution, you can refer to the following sections:

- [Adjust the Volume](../../en/Interactive%20Broadcast/volume_windows.md)
- [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/effect_mixing_windows.md)
- [Adjust the Pitch and Tone](../../en/Interactive%20Broadcast/voice_effect_windows.md)
- [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_windows.md)
