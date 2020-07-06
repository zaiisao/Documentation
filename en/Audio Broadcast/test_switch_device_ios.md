
---
title: Test a Media Device
description: 
platform: iOS
updatedAt: Mon Jul 06 2020 08:01:03 GMT+0800 (CST)
---
# Test a Media Device
## Introduction

To ensure smooth communications, we recommend conducting a media device test before joining a channel to check whether the microphone or camera works properly. This function applies to scenarios that have high-quality requirements, such as online education.

Before joining a channel, you can use the `startEchoTestWithInterval` method to test if the audio devices, such as the microphone and the speaker, are working properly. This article explains how to use this method in your project.

## Implementation

Before proceeding, ensure that you implement a basic call or live interactive streaming in your project. See [Start a Video Call](../../en/Audio%20Broadcast/start_call_ios.md) or [Start Live Interactive Video Streaming](../../en/Audio%20Broadcast/start_live_ios.md) for details.

1. Call `startEchoTestWithIntercal` before joining a channel. You need to set the `interval` parameter in this method to notify the SDK when to report the result of this test. The value range is [2, 10], and the default value is 10 (in seconds).

2. When the echo test starts, let the user speak for a while. If the recording plays back within the set time interval, the audio devices and the network connection are working properly.

3. Once you get the test result, call `stopEchoTest` to stop the current test before joining a channel using joinChannel.

### Sample code

```swift
// Swift
// Start the echo test.
agoraKit.startEchoTestWithInterval(10)

// Wait and check if the user can hear the recorded audio.

// Stop the echo test.
agoraKit.stopEchoTest
```

```objective-c
// Objective-C
// Start the echo test.
[agoraKit startEchoTestWithInterval: 10];

// Wait and check if the user can hear the recorded audio.

// Stop the echo test.
[agoraKit stopEchoTest];
```

### API Reference

- [`startEchoTestWithInterval`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:)
- [`stopEchoTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopEchoTest)

## Considerations

- Once the echo test ends, you must call `stopEchoTest` to stop it. Otherwise, you cannot take another echo test, or enter a call using `joinChannelByToken`.
- In a Live-broadcast channel, only a broadcaster can call `startEchoTestWithInterval`. 
