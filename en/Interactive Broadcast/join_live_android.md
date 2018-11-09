
---
title: Join a Channel
description: 
platform: Android
updatedAt: Fri Nov 09 2018 14:56:04 GMT+0000 (UTC)
---
# Join a Channel
You need to set the channel profile before the app joins a channel.

## Set the channel profile as live broadcast
After initializing RtcEngine, call the `setChannelProfile` method to set the channel profile. RtcEngine applies optimization according to the channel profile.

In the `setChannelProfile` method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: Host and Audience. The host\(broadcaster\) sends and receives audio and video streams while the audience can only receive the audio and video streams.

> -   Call the `setChannelProfile` method before joining a channel.
> -   One engine can be specified one profile only. If you want to switch to another profile, destroy the current RtcEngine using the `destroy` method and create a new RtcEngine before calling the `setChannelProfile` method to set the new channel profile.

```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_LIVE_BROADCASTING);
```

## Join a live broadcast channel
Call the `joinChannel` method to join a channel. 

In the `joinChannel` method:

-   Pass a token that can identify the role and privilege of the user. Set token as null if the security requirements are low. A token is generated at the server of the application. For how to generate a token, see [Security Keys](../../en/Interactive%20Broadcast/token.md).
-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.
-   Pass a uid that can identify the user. Every user in a channel requires a unique uid. If you want to join the same channel on different devices, ensure that different uids are used for each device.

> Once in a channel, a user must call the `leaveChannel` method to exit the current channel before entering another channel.

```
 private void joinChannel() {
    mRtcEngine.joinChannel(null, "demoChannel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

