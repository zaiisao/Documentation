
---
title: Video Stream Fallback
description: 
platform: Web
updatedAt: Sun Sep 29 2019 09:42:28 GMT+0800 (CST)
---
# Video Stream Fallback
## Introduction

The audio and video quality of a live broadcast or a video call deteriorates under poor network conditions. To improve the efficiency of a live broadcast, the `setStreamFallbackOption` method is used for the SDK automatically switch the high-video stream to low-video stream and disbale the video stream under these conditions.


## Implementation

Before proceeding, ensure that you implement a basic live broadcast in your project. See [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_web.md) for details.

Refer to the following steps to set the stream fallback under poor network conditions:

1. After calling the `Stream.init` method successfully, the host calls the `enableDualStreamMode` method to enable [dual stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode).
	> We do not recommend using the track methods ([addTrack](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#addtrack)/[removeTrack](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#removetrack)/[replaceTrack](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#replacetrack)) on dual streams, which might cause different performance in the high-video and low-video streams.

2. Users in the channel call the `setStreamFallbackOption` method to set the subscribed stream fallback under poor network conditions.
	- Set `fallbackType (1)` to only subscribe to the low-video stream from the host under poor network conditions.
	- Set `fallbackType (2)` to susbcribe to the low-video stream or even audio stream from the host under poor network conditions.

3. (Optional) Users in the channel call the `setRemoteVideoStreamType` method and set `streamType (1)` to only subscribe to the low-video stream under poor network conditions.
	
Once the remote media stream switches to the low stream due to poor network conditions, you can monitor the stream switch between a high and low stream in the `Client.on("stream-type-changed")` callback. When the remotely subscribed video stream falls back to audio only or when the audio-only stream switches back to the video stream, the SDK triggers the `Client.on("stream-fallback")` callback. 



### Sample code

```javascript
//Javascript
// Enable the dual-stream mode (local configuration).
client.enableDualStream(function() {
console.log("Enable dual stream success!")
}, function(err) {
console,log(err)
})

// Configuration for the publisher. When the network condition is poor, send audio only. 
client.setStreamFallbackOption(remoteStream, 2)

// Configuration for the subscriber. Try to receive low stream under poor network conditions. When the current network conditions are not sufficient for video streams, receive audio only. 
client.setRemoteVideoStreamType(remoteStream, 1);
```

### API reference

- [`enableDualStreamMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#enabledualstream)
- [`setStreamFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setstreamfallbackoption)
- [`setRemoteVideoStreamType`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setremotevideostreamtype)

## Considerations

- The `enableDualStream` method does not apply to the following scenarios:
  - The stream is created by defining the [`audioSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#audiosource) and [`videoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#videosource) properties.
  - Audio-only mode (audio: true, video: false)
  - Safari browser on iOS
  - Screen-sharing

- Some web browsers may not be fully compatible with dual streams when calling the `setRemoteVideoStreamType` method:
  - Safari on macOS: A high-video stream and a low-video stream share the same frame rate and resolution.
  - Firefox: A low-video stream has a fixed frame rate of 30 fps.
  - Safari on iOS: Safari 11 does not support switching between the two video streams.


