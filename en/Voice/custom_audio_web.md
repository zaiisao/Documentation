
---
title: Custom Audio Source
description: How to use external audio sources for Web SDK
platform: Web
updatedAt: Tue Sep 24 2019 08:17:56 GMT+0800 (CST)
---
# Custom Audio Source
## Introduction

By default, a browser uses the internal audio modules for capturing and rendering during real-time communication. You can use an external audio source and renderer. This page shows how to use the methods provided by Agora SDK to customize the audio source and renderer.

**Customizing the audio source** mainly applies to the following scenarios:

- When the audio source captured by the internal modules do not meet your needs.
- When an app has its own audio module and uses a customized source for code reuse.
- When you need flexible device resource allocation to avoid conflicts with other services.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Voice/web_prepare.md).

### Customize the audio source

You can specify customized audio source by the [`audioSource`](https://docs.agora.io/en/Voice/API%20Reference/web/interfaces/agorartc.streamspec.html#audiosource) property when creating a stream. 

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

> - `MediaStreamTrack` refers to the `MediaStreamTrack` object supported by the browser. See [MediaStreamTrack API](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack) for details.
> - Only supports the Chrome browser.


Agora provides a sample app for customizing the audio/video source and video sink. See [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-VideoSource-Web).

