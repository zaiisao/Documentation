
---
title: Starting a Live Video Broadcast
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:11:19 GMT+0800 (CST)
---
# Starting a Live Video Broadcast
In this quickstart, you will learn how to use the Agora SDK to making a live video broadcast.

## Prerequisites

1.  See [Setting up Your Development Environment](../../en/Quickstart%20Guide/android_video.md) for the prerequisites, environment setup and SDK integration.

2.  See [OpenLive for Android](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-Android) to better understand how a Sample App is created from scratch.


## Quickstart

### Creating an Agora Instance

The following imports define the interface of the Agora API that provides communication functionality:

-   `io.agora.rtc.Constants`
-   `io.agora.rtc.IRtcEngineEventHandler`
-   `io.agora.rtc.RtcEngine`
-   `io.agora.rtc.video.VideoCanvas`

Create a singleton by invoking `create` during initialization. In this method:

-  Pass the Agora App ID. Only Applications with the same App ID can join the same channel.
-  Specify a reference to the activity’s event handler. The Agora API uses events to inform the application about Agora engine runtime events, such as joining or leaving a channel and adding new participants.

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

In this method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: host and audience. The host\(broadcaster\) sends and receives audio and video streams while the audience can only receive the audio and video streams.

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

### Enabling the Video Mode

Call `enableVideo` to enable the video mode. The voice function is enabled by default in the Agora SDK, so you can call this method before or after joining in the channel.

-   If you enable the video mode before joining the channel, you enter directly into a video call.
-   If you enable the video mode after joining the channel, the voice call switches to a video call.


```
mRtcEngine.enableVideo();
```

### Setting the Video Profile

After the video mode is enabled, use `setVideoEncoderConfiguration` to set the video encoder profile.

In this method, secify the video encoding resolution, frame rate bitrate and orientation mode. See descriptions in `setVideoEncoderConfiguration` for more information.

> -   The parameters specified in this method are the maximum values under ideal network conditions. If the video engine cannot render the video using the specified parameters due to poor network conditions, the parameters further down the list are considered until success.
> -   If the device camera does not support the resolution specified by the video profile, the SDK automatically chooses a suitable camera resolution. This does not change the encoder resolution, and encoding will still use the resolution specified in this method .

```
VideoEncoderConfiguration.ORIENTATION_MODE
orientationMode =
VideoEncoderConfiguration.ORIENTATION_MODE.ORIENTATION_MODE_FIXED_PORTRAIT;

    VideoEncoderConfiguration.VideoDimensions dimensions = new VideoEncoderConfiguration.VideoDimensions(360, 640);

    VideoEncoderConfiguration videoEncoderConfiguration = new VideoEncoderConfiguration(dimensions, frameRate, bitrate, orientationMode);

mRtcEngine.setVideoEncoderConfiguration(videoEncoderConfiguration);
```

### Setting the Local Video View

The local video view means the display of the local video streams on the user’s local device.

Call the `setupLocalVideo` method before entering into a channel to bind the application with the video window of the local stream and configure the local video display. This method creates a `surfaceView` object for the video stream by initializing:

-   The Z order media overlay using `surfaceView.setZOrderMediaOverlay`. Setting the parameter to true places the view over of the parent view.
-   The `surfaceView`, adding it to the `local_video_view_container` layout `container`.

To complete the local video setup, pass a new `VideoCanvas` object to mRtcEngine. This binds the video window `surfaceView` and configures the video display settings.

```
 private void setupLocalVideo() {
    FrameLayout container = (FrameLayout) findViewById(R.id.local_video_view_container);
    SurfaceView surfaceView = RtcEngine.CreateRendererView(getBaseContext());
    surfaceView.setZOrderMediaOverlay(true);
    container.addView(surfaceView);
    mRtcEngine.setupLocalVideo(new VideoCanvas(surfaceView, VideoCanvas.RENDER_MODE_ADAPTIVE, 0));
}
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

### Setting the Remote Video View

The remote video view means the display of the remote video streams on the user’s local device.

Call the `setupRemoteVideo` method to set the video display view for the remote user with their guid, and does the following:

-   Gets a reference to the remote video view in the layout.
-   Creates and adds a **View** object to the layout.
-   Creates a **VideoCanvas** and associates the view with it.
-   Tags the **View** with the channel ID.
-   Hides the quick tips.

Users need to pass the UID of the remote user in this method. If the remote UID is unknown to the application, set it later when the application receives the `onUserJoined` event.


```
private void setupRemoteVideo(int uid) {
   FrameLayout container = (FrameLayout) findViewById(R.id.remote_video_view_container);

   if (container.getChildCount() >= 1) {
       return;
   }

   SurfaceView surfaceView = RtcEngine.CreateRendererView(getBaseContext());
   container.addView(surfaceView);
   mRtcEngine.setupRemoteVideo(new VideoCanvas(surfaceView, VideoCanvas.RENDER_MODE_ADAPTIVE, uid));

   surfaceView.setTag(uid);
   View tipMsg = findViewById(R.id.quick_tips_when_use_agora_sdk);
   tipMsg.setVisibility(View.GONE);
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

You can now start a video live broadcast.

The SDK turns on the camera by default, after a user joins a channel. From there, you can invoke:

-  `startPreview`: to enable the preview function.
-   `disableVideo`: to switch to voice-only mode.


## Reference

Followings are part of the Agora APIs that we use to start a video live broadcast:

-   Create an RtcEngine Object: `create`
-   Set the Channel Profile: `setChannelProfile`
-  Create a Renderer View: `createRendererView`
-  Enable the Video Mode: `enableVideo`
-  Set the Local Video View: `setupLocalVideo`
-  Set the Remote Video View: `setupRemoteVideo`
-  Set the Video Profile: `setVideoProfile`
-   Set the User Role`setClientRole`
-  Join a Channel: `joinChannel`


The following functions can be implemented in a live video broadcast:

-   [Hosting In](../../en/Quickstart%20Guide/hostin_android.md)
-   [Hosting In Across Channels](../../en/Quickstart%20Guide/hostin_crosschannel_android.md)
-   [Sending Point-to-point Text and Channel Messages](../../en/Quickstart%20Guide/signal_android-1.md)
-   [Pushing Streams to the CDN](../../en/Quickstart%20Guide/push_stream_android.md)
-   [Recording Voice and Video](../../en/Quickstart%20Guide/recording_voice_video.md)
-   [Advanced: Using Agora Built-in Encryption](../../en/Quickstart%20Guide/encryption_android_agora.md)
-   [Advanced: Implementing Encryption](../../en/Quickstart%20Guide/encryption_ios_agora.md)
-   [Advanced: Modifying Raw Data](../../en/Quickstart%20Guide/rawdata_android.md)

For more added functions, see descriptions of all the Agora APIs in [Interactive Broadcast API](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/index.html).


