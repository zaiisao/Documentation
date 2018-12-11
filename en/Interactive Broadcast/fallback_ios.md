
---
title: Improve Experience Under Poor Network Conditions
description: 
platform: iOS,macOS
updatedAt: Wed Nov 21 2018 10:38:42 GMT+0000 (UTC)
---
# Improve Experience Under Poor Network Conditions
## Introduction

The audio and video quality of a live broadcast or a video call deteriorates under poor network conditions. To improve the efficiency of a live broadcast or a video call, the `setLocalPublishFallbackOption` and `setRemoteSubscribeFallbackOption` methods are used for the SDK to automatically disable the video stream when the network conditions cannot support both audio and video, and re-enable the video when the network conditions improve. The `onLocalPublishFallbackToAudioOnly` or `onRemoteSubscribeFallbackToAudioOnly` callback is triggered when the stream falls back to audio-only or when the stream switches back to the video.

## Implementation


```swift
// Swift
// Enable the dual-stream mode
agoraKit.enableDualStreamMode(true);

// Configurationa for the publisher
// When the network condition is poor, send video stream only. 
agoraKit.setLocalPublishFallbackOption(.audioOnly)

// Configurations for the subscriber.
// Try to receive low stream under poor network conditions. When the current network conditions are not sufficient for video streams, receive audio stream only. 
agoraKit.setRemoteSubscribeFallbackOption(.audioOnly)

// You can switch the remote video stream with a specified uid to low stream. 
// Note that you have to set the remote stream to dual-stream mode before receiving its low stream. 
agoraKit.setRemoteVideoStream(uid, type: .low)
```

```oc
//Objective-C
// Enable the dual-stream mode
agoraKit.enableDualStreamMode(true);

// Configurationa for the publisher
// When the network condition is poor, send video stream only. 
[agoraKit setLocalPublishFallbackOption: AgoraStreamFallbackOptionAudioOnly];

// Configurations for the subscriber.
// Try to receive low stream under poor network conditions. When the current network conditions are not sufficient for video streams, receive audio stream only. 
[agoraKit setRemoteSubscribeFallbackOption: AgoraStreamFallbackOptionAudioOnly];

// You can switch the remote video stream with a specified uid to low stream. 
// Note that you have to set the remote stream to dual-stream mode before receiving its low stream. 
[agoraKit setRemoteVideoStream: uid type:AgoraVideoStreamTypeLow];
```

## API Reference

- [enableDualStreamMode:](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:)
- [setRemoteVideoStream:type:](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVideoStream:type:)
- [setLocalPublishFallbackOption:](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:)
- [setRemoteSubscribeFallbackOption:](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:)


