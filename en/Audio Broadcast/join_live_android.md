
---
title: Join a Channel
description: 
platform: Android
updatedAt: Thu Jul 18 2019 11:47:25 GMT+0800 (CST)
---
# Join a Channel
## Prerequisites

Before joining the channel, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/android_video.md).

When working on a test version of your application, you can generate a temporary token at the [Agora Dashboard](https://dashboard.agora.io/) to join a channel. 

On the **Project Details** page, click **Generate a Temp Token**, enter a channel name, and you will get a temporary token on the **Token** page. 

![](https://web-cdn.agora.io/docs-files/1563113619615)

![](https://web-cdn.agora.io/docs-files/1563113643411)

> - Ensure that you have enabled the App Certificate of the project before clicking **nerate a Temp Token** . See [Enablet the App Certificate](#appcertificate).
> - A temp token applies to scenarios with low security requirements. For the production environment, we recommend using a token generated at your server.

## Implementation
You need to set the channel profile before the app joins a channel.

### Set the channel profile as Live Broadcast
After initializing RtcEngine, call the `setChannelProfile` method to set the channel profile. RtcEngine applies optimization according to the channel profile.

In the `setChannelProfile` method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: 

- The host\(broadcaster\) sends and receives audio and video streams
- The audience receives the audio and video streams.

> -   Call the `setChannelProfile` method before joining a channel.
> -   One engine uses one profile only. If you want to switch to another profile, destroy the current RtcEngine using the `destroy` method and create a new RtcEngine before calling the `setChannelProfile` method to set the new channel profile.

```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_LIVE_BROADCASTING);
```

### Join a live broadcast channel
Call the `joinChannel` method to join a channel. 

In the `joinChannel` method:

-   Pass a token that identifies the role and privilege of the user. A Temp Token can be used at the testing stage. For the production environment, we recommend using a Token generated at your server. For how to generate a token, see [Security Keys](../../en/Audio%20Broadcast/token.md). 
-   Pass a channel ID that identifies the channel. Users that input the same channel ID enter into the same channel.
-   Pass a uid that identifies the user. Each user in a channel requires a unique uid. If you want to join the same channel on different devices, ensure that different uids are used for each device.

> Once in a channel, a user must call the `leaveChannel` method to exit the current channel before entering another channel.

```
 private void joinChannel() {
    mRtcEngine.joinChannel("token", "demoChannel1", "Extra Optional Data", 0); // If you do not specify the uid, Agora will assign one.
}
```

## Next Steps
You are in the channel and can start a live broadcast with the following steps:

- [Switch the Client Role](../../en/Interactive%20Broadcast/role_android.md)
- [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_android_live.md)

For more functions, you can refer to the following sections:

- [Adjust the Volume](../../en/Interactive%20Broadcast/volume_android.md)
- [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/effect_mixing_android.md)
- [Use In-Ear Monitoring](../../en/Interactive%20Broadcast/in-ear_android.md)
- [Adjust the Pitch and Tone](../../en/Interactive%20Broadcast/voice_effect_android.md)
- [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_android.md)

