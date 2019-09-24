
---
title: Custom Video Source and Renderer
description: How to use external audio/video sources for Web SDK
platform: Web
updatedAt: Tue Sep 24 2019 08:45:00 GMT+0800 (CST)
---
# Custom Video Source and Renderer
## Introduction

By default, a browser uses the internal audio and video modules for capturing and rendering during real-time communication. You can use an external audio or video source and renderer. This page shows how to use the methods provided by Agora Web SDK to customize the audio and video source and renderer.

**Customizing the audio and video source and renderer** mainly applies to the following scenarios:

- When the audio or video source captured by the internal modules do not meet your needs. For example, you need to process the captured video frame with a preprocessing library for image enhancement.
- When an app has its own audio or video module and uses a customized source for code reuse.
- When you want to use a non-camera source, such as recorded screen data.
- When you need flexible device resource allocation to avoid conflicts with other services.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md).

### Customize the audio/video source

You can specify customized audio/video sources by the [`audioSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#audiosource) and [`videoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#videosource) properties when creating a stream. 

For example, you can use the `mediaStream` method to get the audio and video tracks from `MediaStreamTrack`, and then set `audioSource` and `videoSource`:

```javascript
navigator.mediaDevices.getUserMedia(
    {video: true, audio: true}
).then(function(mediaStream){
    var videoSource = mediaStream.getVideoTracks()[0];
    var audioSource = mediaStream.getAudioTracks()[0];
    // After processing videoSource and audioSource
    var localStream = AgoraRTC.createStream({
        video: true,
        audio: true,
        videoSource: videoSource,
        audioSource: audioSource
    });
    localStream.init(function(){
        client.publish(localStream, function(e){
            //...
        });
    });
});
```

> - `MediaStreamTrack` refers to the `MediaStreamTrack` object supported by the browser. See [MediaStreamTrack API](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack) for details.
> - Only supports the Chrome browser.

### Customize the video renderer

Call the [`Stream.getVideoTrack`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getvideotrack) method and attach the track to the local canvas.

Agora provides a sample app for customizing the video source and video sink. See [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-VideoSource-Web).

