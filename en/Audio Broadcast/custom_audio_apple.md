
---
title: Custom Audio Source and Renderer
description: 
platform: iOS,macOS
updatedAt: Mon Jul 06 2020 10:35:12 GMT+0800 (CST)
---
# Custom Audio Source and Renderer
## Introduction

Before reading this article, you need to undersand two concepts: audio source and audio sink. Audio source is where audio frames come from, such as a microphone and a recorder. Audio sink is where audio frames go, such as a speaker and a earpiece.

By default, the Agora SDK uses default audio and video modules for capturing and rendering in real-time communications. 

However, the default modules might not meet your development requirements, such as in the following scenarios:

- Your app has its own audio or video module.
- You want to use a non-camera source, such as recorded screen data.
- You need to process the captured video with a pre-processing library for functions such as image enhancement.
- You need flexible device resource allocation to avoid conflicts with other services.

This article tells you how to use the Agora Native SDK to customize the audio source and sink.

## Implementation

Before customizing the audio source or sink, ensure that you implement the basic real-time communication functions in your project. For details, see the following documents:
- iOS: [Start a Call](../../en/Audio%20Broadcast/start_call_ios.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_ios.md)
- macOS: [Start a Call](../../en/Audio%20Broadcast/start_call_mac.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_mac.md)

### Custom audio source

Refer to the following steps to customize the audio source in your project:

1. Call the `enableExternalAudioSourceWithSampleRate` method to enable the external audio source before joining a channel.
2. Record and process the audio data on your own.
3. Send the audio data back to the SDK using the `pushExternalAudioFrameRawData` or `pushExternalAudioFrameSampleBuffer` method according to the format of the audio data..

**API call sequence**

Refer to the following diagram to customize the audio source in your project.

![](https://web-cdn.agora.io/docs-files/1569380674526)

**Sample code**

Refer to the following code to customize the audio source in your project.

```swift
// Swift
// Push the audio frame in the rawData format.
agoraKit.pushExternalAudioFrameRawData("your rawData", samples: "per push samples", timestamp: 0)

// Push the audio frame in the CMSampleBuffer format.
agoraKit.pushExternalAudioFrameSampleBuffer("your CMSampleBuffer")
```

```objective-c
// Objective-C
// Push the audio frame in the rawData format.
[agoraKit pushExternalAudioFrameRawData: "your rawData" samples: "per push samples", timestamp: 0];

// Push the audio frame in the CMSampleBuffer format.
[agoraKit pushExternalAudioFrameSampleBuffer: "your CMSampleBuffer"];
```

**API Reference**
- [`enableExternalAudioSourceWithSampleRate`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSourceWithSampleRate:channelsPerFrame:)
- [`disableExternalAudioSource`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSource)
- [`pushExternalAudioFrameRawData`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pushExternalAudioFrameRawData:samples:timestamp:)
- [`pushExternalAudioFrameSampleBuffer`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pushExternalAudioFrameSampleBuffer:)

### Custom audio sink

Refer to the following steps to customize the audio sink in your project:

1. Call the `enableExternalAudioSink` method to enable the external audio sink before joining a channel.
2. After you join the channel, call the `pullPlaybackAudioFrameRawData` or `pullPlaybackAudioFrameSampleBufferByLengthInByte` method to get the remote audio data according to the format of the audio data.
3. Play the remote audio data on your own.

**API call sequence**

Refer to the following diagram to customize the audio sink in your project.

![](https://web-cdn.agora.io/docs-files/1569380959511)

**Sample code**

Refer to the following code to customize the audio sink in your project.

```swift
// Swift
// Pull the audio frame in the rawData format
agoraKit.pullPlaybackAudioFrameRawData("your rawData", lengthInByte: "data length in byte of the external audio data")

// Pull the audio frame in the CMSampleBuffer format
agoraKit.pullPlaybackAudioFrameSampleBufferByLengthInByte(lengthInByte: "data length in byte of the external audio data")
```

```objective-c
// Objective-C
// Pull the audio frame in the rawData format
[agoraKit pullPlaybackAudioFrameRawData: "your rawData" lengthInByte: "data length in byte of the external audio data"];

// Pull the audio frame in the CMSampleBuffer frame
[agoraKit pullPlaybackAudioFrameSampleBufferByLengthInByte: lengthInByte: "data length in byte of the external audio data"];
```

**API reference**

- [`enableExternalAudioSink`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)

## Demo project

We provide open-source demo projects on GitHub. You can try the demo and refer to the source code.
- iOS: [AgoraAudioIO-iOS](https://github.com/AgoraIO/Advanced-Audio/tree/master/iOS%26macOS/AgoraAudioIO-Objective-C/AgoraAudioIO-iOS) for Objective-C. Refer to the code in [`RoomViewController.m`](https://github.com/AgoraIO/Advanced-Audio/blob/master/iOS%26macOS/AgoraAudioIO-Objective-C/AgoraAudioIO-iOS/RoomViewController.m).
- macOS: [AgoraAudioIO-macOS](https://github.com/AgoraIO/Advanced-Audio/tree/master/iOS%26macOS/AgoraAudioIO-Objective-C/AgoraAudioIO-macOS) for Objective-C. Refer to the code in [`RoomViewController.m`](https://github.com/AgoraIO/Advanced-Audio/blob/master/iOS%26macOS/AgoraAudioIO-Objective-C/AgoraAudioIO-macOS/RoomViewControllerMac.m).

## Consideration

Customizing the audio source and sink requires you to manage audio data recording and playback on your own.

- When customizing the audio source, you need to record and process the audio data on your own.
- When customizing the audio sink, you need to process and play back the audio data on your own.
