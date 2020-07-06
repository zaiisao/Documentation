
---
title: Test a Media Device
description: 
platform: macOS
updatedAt: Mon Jul 06 2020 08:05:32 GMT+0800 (CST)
---
# Test a Media Device
## Introduction

To ensure smooth communications, we recommend conducting a media device test before joining a channel to check whether the microphone or camera works properly. This function applies to scenarios that have high-quality requirements, such as online education.

## Implementation

Before proceeding, ensure that you have implemented basic real-time functions in your project. See [Start a  Call](../../en/Audio%20Broadcast/start_call_mac.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_mac.md) for details.

- Choose either of the following ways to test the audio devices:
	- Call the `startEchoTestWithInterval` method to test if the audio devices and network connection are working properly.
	- Call the `startRecordingDeviceTest` method to test the audio recording devices, and call the `startPlaybackDeviceTest` method to test the audio playback devices.
	- Call the `startAudioDeviceLoopbackTest` method to test the audio device loopback (including the recording and playback devices).
- Call the `startCaptureDeviceTest` method to test the video capture devices.

<div class="alert note">Test the devices before joining a channel.</div>

### Echo test

Call the `startEchoTestWithInterval` method to test if the audio devices, such as the microphone and the speaker, are working properly.

To consuct the test, call `startEchoTestWithInterval`, and set the `interval` parameter in this method to notify the SDK when to report the result of this test. The user speaks, and if the recording plays back within the set time interval, the audio devices and the network connection are working properly.

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

### Recording device test

Call the `startRecordingDeviceTest` method to test whether the local audio recording device, such as the microphone, is working properly.

To conduct the test, the user speaks, and the SDK reports the audio volume information in the `reportAudioVolumeIndication` callback. A UID of 0 indicates the local user.

When the test finishes, call the `stopRecordingDeviceTest` method to stop the current test.

```swift	
// swift
// Starts the recording device test.
agoraKit.startRecordingDeviceTest(1000)
	
// Stops the recording device test.
agoraKit.stopRecordingDeviceTest()
```

```oc
// objective-c
// Starts the recording device test.
[agoraKit startRecordingDeviceTest: 1000];

// Stops the recording device test.
[agoraKit stopRecordingDeviceTest];
```

### Audio playback device test

Call the `startPlaybackDeviceTest` method to test whether the local audio playback device, such as the speaker, is working properly.

To conduct the test, specify an audio file for playback. If you can hear the audio file, the audio playback device works properly.

When the test finishes, call the `stopPlaybackDeviceTest` method to stop the current test.

```swift
// swift
// Starts the playback device test.
agoraKit.startPlaybackDeviceTest("audio file path")
	
// Stops the playback device test.
agoraKit.stopPlaybackDeviceTest()
```

```oc
// objective-c
// Starts the playback device test.
[agoraKit startPlaybackDeviceTest: @"audio file path"];

// Stops the playback device test.
[agoraKit stopPlaybackDeviceTest];
```

### Audio device loopback test

Call the `startAudioDeviceLoopbackTest` method to test whether the local audio devices, including the microphones and speakers, are working properly.

To conduct the test, the user speaks, then the microphone captures the local audio and plays it through the speaker. The SDK reports the audio volume information in the `reportAudioVolumeIndication` callback. A UID of 0 indicates the local user.

When the test finishes, call the `stopAudioDeviceLoopbackTest` method to stop the current test.

### Video capture device test

After calling the `enableVideo` method, call the `startCaptureDeviceTest` method to test whether the local video devices, such as the camera, are working properly.

To conduct the test, specify a window view that displays the image. If you can see the local video view, the video devices work properly.

When the test finishes, call the `stopCaptureDeviceTest` method to stop the current test.

```swift
// swift
// Starts the video capture device test.
agoraKit.startCaptureDeviceTest("view window")

// Stops the video capture device test.
agoraKit.stopCaptureDeviceTest
```

```objective-c
// objective-c
// Starts the video capture device test.
[agoraKit startCaptureDeviceTest: "view window"];

// Stops the video capture device test.
[agoraKit stopCaptureDeviceTest];
```

### API Reference

* [`startEchoTestWithInterval`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:)
* [`stopEchoTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopEchoTest)
* [`startRecordingDeviceTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest)
* [`stopRecordingDeviceTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest)
* [`startPlaybackDeviceTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)
* [`startCaptureDeviceTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startCaptureDeviceTest:)
* [`stopCaptureDeviceTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopCaptureDeviceTest)
* [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioDeviceLoopbackTest:)
* [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioDeviceLoopbackTest)

## Considerations
- Ensure that you call the corresponding `stop` methods after the test ends. Otherwise, you cannot take another test, or enter a call using `joinChannelByToken`. 
- 
If the input device fails to initialize, check the error message in [Developer Center](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Constants/AgoraErrorCode.html).
