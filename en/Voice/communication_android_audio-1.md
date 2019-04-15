
---
title: Making a Voice Call
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:06:40 GMT+0800 (CST)
---
# Making a Voice Call
In this quickstart, you will learn how to use the Agora SDK to make a voice call.

## Prerequisites

1.  See [Setting up Your Development Environment](../../en/Voice/android_audio.md) for the prerequisites, environment setup and SDK integration.

2.  See [Android Voice Tutorial](https://github.com/AgoraIO/Basic-Audio-Call/tree/master/One-to-One-Voice/Agora-Android-Voice-Tutorial-1to1) to better understand how a Sample App is created from scratch.


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

After initializing the Agora Engine, call the `setChannelProfile` method to set the channel profile. Agora Engine applies optimization according to the channel profile.

In this methoid, set the channel profile as Communication. This profile applies to voice or video calls such as one-to-one or group calls, where all users in the channel can talk freely. This is the default setting.

> -   Call this method before joining the channel.
> -   One Engine can be specified one profile only. If you want to switch to another profile, destroy the current Engine using `destroy` and create a new one before calling this method to set the new channel profile.


```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_COMMUNICATION);
```

### Joining a Channel

Now, you can join a voice call channel. Call the `joinChannel` method to join a channel. Users in the same channel can talk to each other, and multiple users in the same channel can initiate a group chat. In this method:

-   Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the Application. For how to generate a Token, see [Security Keys](../../en/Video/token.md).
-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.
-   Pass a UID that can identify the user. Every user in the channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.

> Once in a call, a user must call the `leaveChannel` method to exit the current call before entering another channel.

```
 private void joinChannel() {
    mRtcEngine.joinChannel(null, "demoChannel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

### Ending a Call

When a voice call ends, call the `leaveChannel` method to leave the channel.

This method releases all resources related to the call. When the user leaves the channel, the SDK triggers the `onLeaveChannel` callback.

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> If the `destroy` method is called immediately after `leaveChannel`, the process will be interrupted, and the SDK will not trigger the `onLeaveChannel` callback.

### Conclusion

You can now start a one-to-one or group voice call. The only difference between a one-to-one and group call is the number of people joining the channel.

## Reference

Followings are part of the Agora APIs that we use to make a voice call:

-   Create an RtcEngine Object: `create`
-   Set the Channel Profile: `setChannelProfile`
-   Join a Channel: `joinChannel`
-   Leave a Channel: `leaveChannel`


Refer to the following links to add more functions to a voice call:

-   [Recording Voice and Video](../../en/Recording/recording_voice_video.md)

-   [Using Agora Built-in Encryption](../../en/Voice/encryption_android_agora.md)

-   [Modifying Raw Data](../../en/Voice/rawdata_android.md)


For more added functions, see descriptions of all the Agora APIs in [Voice Call API](https://docs.agora.io/en/Voice/API%20Reference/java/index.html).

