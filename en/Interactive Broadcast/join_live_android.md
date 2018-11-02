
---
title: Join a Channel
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:14:58 GMT+0000 (UTC)
---
# Join a Channel
Set the channel profile before the App joins a channel.

## Set the channel profile as live broadcast
After initializing RtcEngine, call `setChannelProfile` method to set the channel profile. RtcEngine applies optimization according to the channel profile.

In this method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: host and audience. The host\(broadcaster\) sends and receives audio and video streams while the audience can only receive the audio and video streams.

> -   Call this method before joining the channel.
> -   One Engine can be specified one profile only. If you want to switch to another profile, destroy the current Engine using `destroy` and create a new one before calling this method to set the new channel profile.

```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_LIVE_BROADCASTING);
```

## Join a live broadcast channel
Call the `joinChannel` method to join a channel. 

In this method:

-   Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Interactive%20Broadcast/token.md).
-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.
-   Pass a UID that can identify the user. Every user in a channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.

> Once in a channel, a user must call the `leaveChannel` method to exit the current channel before entering another channel.

```
 private void joinChannel() {
    mRtcEngine.joinChannel(null, "demoChannel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

