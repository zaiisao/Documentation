
---
title: Share the Screen
description: 
platform: Android
updatedAt: Fri Dec 07 2018 19:55:59 GMT+0000 (UTC)
---
# Share the Screen
## Introduction
During a video call or live broadcast, **sharing the screen** enhances communication by bringing whatever is on the speaker's screen to the other speakers or audience in the channel.

Screen share has extensive application in the following scenarios:

- For a video conference, the speaker can share the image of the local file, web page, and PPT with other users in the channel.
- For an online class, the teacher can share the image of the slides or notes with the students.

## Implementation

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/android_video.md) for details.

Agora SDK does not provide specific APIs for screen sharing on the Android platform. With the system API provided by Android, however, you can enable screen sharing with the following steps:
- Use MediaProjection/VirtualDisplay to get the screen image data.
- Maintain an OpenGL environment, screate a SurfaceView and pass the View to VirtualDisplay as the recipient of the screen image data.
- Take the image data retrived from the SurfaceView callback as the external video source. Call `pushExternalVideoFrame` to transfer the image data to the remote user.

```java
MediaProjectionManager projectManager = (MediaProjectionManager) mContext.getSystemService(
Context.MEDIA_PROJECTION_SERVICE);

// create the intent for screen capture. Call startActivityForResult() to use the sharing funciton
Intent intent = projectManager.createScreenCaptureIntent();
startActivityForResult(intent);

MediaProjection projection;
VirtualDisplay display;

// Override and implement onActivityResult() method of the Activity where you just called startActivityForResult()
@Override
onActivityResult(int requestCode, int resultCode, Intent resuleData) {
    projection = projectManager.getMediaProjection(resultCode, resultData);
    display = projection.createVirtualDisplay(name, width, height, dpi, flags, surface, callback, handler);
}

// The texture retrieved from the Surface will be sent by the SDK
rtcEngine.pushExternalVideoFrame(new AgoraVideoFrame(...));

// Stop screen sharing
projection.stop();
```

You can also refer to the [Agora Screen Sharing](https://github.com/AgoraIO/Advanced-Video/tree/master/Screensharing/Agora-Screen-Sharing-Android#agora-screen-sharing-android) Sample Code to implement screen sharing on Android.

## Considerations

- Android API Level 21+ is required for APIs like MeidaProjection. See [Google MediaProjection API  Reference](https://developer.android.com/reference/android/media/projection/MediaProjection) for details.
- The code snippets in this article is the abridged version. See the [Sample Code](https://github.com/AgoraIO/Advanced-Video/tree/master/Screensharing/Agora-Screen-Sharing-Android#agora-screen-sharing-android) for details on the design and implementation.
