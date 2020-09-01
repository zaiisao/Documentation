
---
title: Custom Audio Source
description: How to use external audio sources for Web SDK
platform: Web
updatedAt: Tue Sep 01 2020 07:23:48 GMT+0800 (CST)
---
# Custom Audio Source
## Introduction

By default, the Agora SDK uses default audio and video modules for capturing and rendering in real-time communications. 

However, the default modules might not meet your development requirements, such as in the following scenarios:

- Your app has its own audio or video module.
- You want to use a non-camera source, such as recorded screen data.
- You need to process the captured video with a pre-processing library for functions such as image enhancement.
- You need flexible device resource allocation to avoid conflicts with other services.

This page explains how to customize the audio source with the Agora Web SDK.

## Implementation

Before customizing the audio source, ensure that you have implemented the basic real-time communication functions. For details, see [Start a call](../../en/Voice/start_call_web.md) or [Start Live Interactive Streaming](../../en/Voice/start_live_web.md).

When creating a stream with the `createStream` method, you can specify the customized audio source by the [`audioSource`](https://docs.agora.io/en/Voice/API%20Reference/web/interfaces/agorartc.streamspec.html#audiosource) property when creating a stream.
For example, you can use the `mediaStream` method to get the audio track from `MediaStreamTrack`, and then set `audioSource`:

```javascript
navigator.mediaDevices.getUserMedia(
    {video: false, audio: true}
).then(function(mediaStream){
    var audioSource = mediaStream.getAudioTracks()[0];
    // After processing audioSource
    var localStream = AgoraRTC.createStream({
        video: false,
        audio: true,
        audioSource: audioSource
    });
    localStream.init(function(){
        client.publish(localStream, function(e){
            //...
        });
    });
});
```

<div class="alert info"><code>MediaStreamTrack</code> refers to the <code>MediaStreamTrack</code> object supported by the browser. See <a href="https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack">MediaStreamTrack API</a> for details.</div>

We also provide an open-source [AgoraAudioIO-Web-Webpack](https://github.com/AgoraIO/Advanced-Audio/tree/master/Web/AgoraAudioIO-Web-Webpack) demo project on GitHub. You can try the demo, or view the source code in the [rtc-client.js](https://github.com/AgoraIO/Advanced-Audio/blob/master/Web/AgoraAudioIO-Web-Webpack/src/rtc-client.js) file.

