
---
title: Starting a Live Voice Broadcast
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:07:24 GMT+0000 (UTC)
---
# Starting a Live Voice Broadcast
In this quickstart, you will learn how to use the Agora SDK to making a live voice broadcast.

## Prerequisites

1.  See [Setting up Your Development Environment](../../en/Quickstart%20Guide/android_audio.md) for the prerequisites, environment setup and SDK integration.

2.  See [OpenLive Voice Only for Android](https://github.com/AgoraIO/Basic-Audio-Broadcasting/tree/master/OpenLive-Voice-Only-Android) to better understand how a Sample App is created from scratch.


## Quickstart

### Creating an Agora Instance

The following imports define the interface of the Agora API that provides communication functionality:

-   `io.agora.rtc.Constants`
-   `io.agora.rtc.IRtcEngineEventHandler`
-   `io.agora.rtc.RtcEngine`

Create a singleton by invoking `create` during initialization. In this method:

-   Pass the Agora App ID. Only Applications with the same App ID can join the same channel.
-   Specify a reference to the activity’s event handler. The Agora API uses events to inform the application about Agora engine runtime events, such as joining or leaving a channel and adding new participants.

```
import io.agora.rtc.Constants;
import io.agora.rtc.IRtcEngineEventHandler;
import io.agora.rtc.RtcEngine;

...

private void initializeAgoraEngine() {
    try {
        mRtcEngine = RtcEngine.create(getBaseContext(), getString(R.string.agora_app_id), mRtcEventHandler);
    } catch (Exception e) {
        Log.e(LOG_TAG, Log.getStackTraceString(e));

        throw new RuntimeException("NEED TO check rtc sdk init fatal error\n" + Log.getStackTraceString(e));
    }
}
```

### Setting the Channel Profile

After initializing RtcEngine, call `setChannelProfile` method to set the channel profile. RtcEngine applies optimization according to the channel profile.

In this method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: host and audience. The host\(broadcaster\) sends and receives audio streams while the audience can only receive the audio streams.

> -   Call this method before joining the channel.
> -   One Engine can be specified one profile only. If you want to switch to another profile, destroy the current Engine using `destroy` and create a new one before calling this method to set the new channel profile.


```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_LIVE_BROADCASTING);
```

### Setting the User Role

The channel includes two user roles once you set the channel profile as live broadcast. Call `setClientRole` method to set the user roles according to the needs. The differences of the two roles are:

-   Host\(Broadcaster\): A host can receive and publish the voice streams. The host can also interact with other hosts using voice.
-   Audience: An audience can only hear the host\(s\).

> Make sure that this method is called before joining a live broadcast channel. You can also call this method during a live broadcast to switch the user role.

```
mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
```

### Joining a Channel

Now, you can join a live broadcast channel. Call the `joinChannel` method to join a channel. Hosts in the same channel can talk to each other, and audiences can hear the host\(s\).

In this method:

-   Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Agora%20Platform/token.md).
-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.
-   Pass a UID that can identify the user. Every user in a channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.

> Once in a live broadcast channel, a user must call the `leaveChannel` method to exit the current channel before entering another channel.

```
 private void joinChannel() {
    mRtcEngine.joinChannel(null, "demoChannel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

### Ending a Live Broadcast

The `leaveChannel` method allows a host to leave a channel to end a live broadcast.

This method releases all resources related to the live broadcast. When the user leaves the channel, the SDK triggers the `onLeaveChannel` callback.

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> If the `destroy` method is called immediately after leaveChannel, the process will be interrupted, and the SDK will not trigger the onLeaveChannel callback.

### Conclusion

You can now start a voice live broadcast.

## Reference

Followings are part of the Agora APIs that we use to start a voice live broadcast:

-   Create an RtcEngine Object: `create`
-   Set the Channel Profile: `setChannelProfile`
-   Set the User Role: `setClientRole`
-   Join a Channel: `joinChannel`

Refer to the following links to add more functions to a live broadcast:

-   [Hosting In](../../en/Quickstart%20Guide/hostin_android.md)

-   [Sending Point-to-point Text and Channel Messages](../../en/Quickstart%20Guide/signal_android-1.md)

-   [Recording Voice and Video](../../en/Quickstart%20Guide/recording_voice_video.md)

-   [Using Agora Built-in Encryption](../../en/Quickstart%20Guide/encryption_android_agora.md)

-   [Implementing Encryption](../../en/Quickstart%20Guide/encryption_ios_agora.md)

-   [Modifying Raw Data](../../en/Quickstart%20Guide/rawdata_android.md)


For more added functions, see descriptions of all the Agora APIs in [Interactive Broadcast API](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/index.html).


