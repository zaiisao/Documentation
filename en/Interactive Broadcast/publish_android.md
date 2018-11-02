
---
title: Publish and Subscribe to Streams
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:08:26 GMT+0000 (UTC)
---
# Publish and Subscribe to Streams
##  Enable the video mode
Call `enableVideo` to enable the video mode. The voice function is enabled by default in the Agora SDK, so you can call this method before or after joining in the channel.

-   If you enable the video mode before joining the channel, you enter directly into a video call.
-   If you enable the video mode after joining the channel, the voice call switches to a video call.


```
mRtcEngine.enableVideo();
```

## Set the video profile
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

## Set the local video view
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

## Set the remote video view
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
