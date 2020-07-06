
---
title: Video Stream Fallback
description: 
platform: iOS,macOS
updatedAt: Mon Jul 06 2020 08:14:45 GMT+0800 (CST)
---
# Video Stream Fallback
## Introduction

The audio and video quality of live interactive video streaming or a video call deteriorates under poor network conditions. To improve the efficiency of live interactive video streaming or a video call, the `setLocalPublishFallbackOption` and `setRemoteSubscribeFallbackOption` methods are used for the SDK to automatically switch the high-video stream to low-video stream and disable the video stream under these conditions.


## Implementation

Before proceeding, ensure that you implement live interactive video streaming or a video call in your project. See [Start Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_ios.md) or [Start a Call](../../en/Interactive%20Broadcast/start_call_ios.md) for details.

Refer to the following steps to set the stream fallback under poor network conditions:

1. After joining the channel, the publisher calls the `enableDualStreamMode` method to enable [dual stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode).
2. The publisher calls the `setLocalPublishFallbackOption` method and set `AudioOnly (2)` to enable the fallback option for the locally published media stream based on the network conditions. The SDK will disable the upstream video but enable audio only when the network conditions deteriorate and cannot support both video and audio streams.
	> Agora does not recommend using this method for CDN live streaming, because the remote CDN live user will have a noticeable lag when the locally published video stream falls back to `audio only`.
	
	When the local video stream falls back to `audio only` or when the audio-only stream switches back to the video stream, the SDK triggers the `didLocalPublishFallbackToAudioOnly` callback to report current stream state to the local publisher.
3. The subscriber in the channel calls the `setRemoteSubscribeFallbackOption` method to set the subscribed stream fallback under poor network conditions.
	- Set `StreamLow (1)` to only subscribe to the low-video stream from the publisher under poor network conditions.
	- Sets`AudioOnly (2)` to subscribe to the low-video stream or even audio stream from the publisher under poor network conditions.
	
	Once the remote media stream switches to the low stream due to poor network conditions, you can monitor the stream switch between a high and low stream in the `remoteVideoStats` callback. When the remotely subscribed video stream falls back to `audio only` or when the audio-only stream switches back to the video stream, the SDK triggers the `didRemoteSubscribeFallbackToAudioOnly` callback. 


### Sample code

```swift
//Swift
// Enable the dual-stream mode (Configuration for the local).
agoraKit.enableDualStreamMode(true)

// Configurations for the publisher. When the network condition is poor, send audio stream only. 
agoraKit.setLocalPublishFallbackOption(.audioOnly)

// Configurations for the subscriber. Try to receive low stream under poor network conditions. When the current network conditions are not sufficient for video streams, receive audio stream only. 
agoraKit.setRemoteSubscribeFallbackOption(.audioOnly)
```

```objective-c
//Objective-C
// Enable the dual-stream mode (Configuration for the local).
[agoraKit.enableDualStreamMode: YES];

// Configuration for the publisher. When the network condition is poor, send audio stream only. 
[agoraKit setLocalPublishFallbackOption: AgoraStreamFallbackOptionAudioOnly];

// Configuration for the subscriber. Try to receive low stream under poor network conditions. When the current network conditions are not sufficient for video streams, receive audio stream only. 
[agoraKit setRemoteSubscribeFallbackOption: AgoraStreamFallbackOptionAudioOnly];
```

### API reference

- [`enableDualStreamMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:)
- [`didLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalPublishFallbackToAudioOnly:)
- [`didRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRemoteSubscribeFallbackToAudioOnly:byUid:)
- [`remoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStats:)
