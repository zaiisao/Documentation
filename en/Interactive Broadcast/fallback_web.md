
---
title: Stream Fallback
description: 
platform: Web
updatedAt: Thu Sep 26 2019 08:41:45 GMT+0800 (CST)
---
# Stream Fallback
## Introduction

The audio and video quality of a live broadcast or a video call deteriorates under poor network conditions. To improve the user experience in poor network conditions,  Agora provides two optimization methods when the publisher enables the dual-stream mode:

- Sets the stream fallback option. Use the `setStreamFallbackOption` method to set the stream fallback option for the subscriber:
  - Under poor network conditions, the SDK automatically subscribes to the low-video stream. When the video-stream type changes, the SDK triggers the `stream-type-changed` callback.
  - Under poor network conditions, the SDK first subscribes to the low-video stream (of lower resolution and lower bitrate), but if the network conditions do not allow displaying the video, the SDK receives audio only. When the stream falls back or recovers, the SDK triggers the `stream-fallback` callback.
- Sets the remote video-stream type. Under poor network conditions, you can manually switch from the high-video stream to the low-video stream by calling the `setRemoteVideoStreamType` method. If the video-stream type changes, the SDK triggers the `stream-type-changed` callback.

## Implementation

For the publisher:

```javascript
// Enables the dual-stream mode.
client.enableDualStream(function() {
    console.log("Enable dual stream success!")
    }, function(err) {
        console,log(err)
})
```

For the subscriber:

- If you want to set the stream fallback option, do the following:

```javascript
// Under poor network conditions, the SDK may subscribe to the low-video stream first, but if the network conditions do not allow displaying the video, the SDK receives audio only.
client.setStreamFallbackOption(remoteStream, 2)
```

- If you want to set the remote video-stream type, do the following:

```javascript
// Switch the high-video stream to the low-video stream.
client.setRemoteVideoStreamType(remoteStream, 1)
```

### API Reference

- [`enableDualStreamMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#enabledualstream)
- [`setStreamFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setstreamfallbackoption)
- [`setRemoteVideoStreamType`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setremotevideostreamtype)

### Considerations

- The `enableDualStream` method does not apply to the following scenarios:
  - The stream is created by defining the [`audioSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#audiosource) and [`videoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#videosource) properties.
  - Audio-only mode (audio: true, video: false)
  - Safari browser on iOS
  - Screen-sharing

- Some web browsers may not be fully compatible with dual streams when calling the `setRemoteVideoStreamType` method:
  - Safari on macOS: A high-video stream and a low-video stream share the same frame rate and resolution.
  - Firefox: A low-video stream has a fixed frame rate of 30 fps.
  - Safari on iOS: Safari 11 does not support switching between the two video streams.


