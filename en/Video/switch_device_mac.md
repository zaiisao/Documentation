
---
title: Test or Select a Media Device
description: 
platform: macOS
updatedAt: Wed Jul 17 2019 07:50:24 GMT+0800 (CST)
---
# Test or Select a Media Device
## Introduction

To ensure smooth communications, we recommend conducting a media device test before joining a channel to check whether the microphone or camera works properly. This function applies to scenarios that have high-quality requirements, such as online education.

## Implementation

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

### Playback device test

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

### Video capture device test

After calling the `enableVideo` method, call the `startCaptureDeviceTest` method to test whether the local video devices, such as the camera, is working properly.

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

* [`startRecordingDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest)
* [`stopRecordingDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest)
* [`startPlaybackDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)
* [`startCaptureDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startCaptureDeviceTest:)
* [`stopCaptureDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopCaptureDeviceTest)

## Considerations

If the input device fails to initialize, check the error message in [Developer Center](https://docs.agora.io/en/Video/API%20Reference/oc/Constants/AgoraErrorCode.html).
