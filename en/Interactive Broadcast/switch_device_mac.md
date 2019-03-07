
---
title: Test or Select a Media Device
description: 
platform: macOS
updatedAt: Thu Mar 07 2019 02:49:58 GMT+0000 (UTC)
---
# Test or Select a Media Device
## Introduction

Agora supports the media device test and selection feature, allowing you to check if a camera or an audio device (a headset, microphone, or speaker) works or connects properly to the SD-RTN. For example, in an audio device test, if a user's recorded voice is replayed in 10 seconds, then the system's audio device works and is connected properly to the SD-RTN.

You can use the media device test and selection feature in the following scenarios:

- A host self-checks before starting a live broadcast.
- An online user checks if the media device works.

## Implementation

### Microphone Test

```swift	
// Starts the microphone test.
agoraKit.startRecordingDeviceTest(1000)
	
// Stops the microphone test.
agoraKit.stopRecordingDeviceTest()
```

```oc
[agoraKit startRecordingDeviceTest: 1000];

[agoraKit stopRecordingDeviceTest];
```



### External Playback Device Test

```swift
// Starts a playback device test.
agoraKit.startPlaybackDeviceTest("audio file path")
	
// Stops a playback device test.
agoraKit.stopPlaybackDeviceTest()
```

```oc
[agoraKit startPlaybackDeviceTest: @"audio file path"];

[agoraKit stopPlaybackDeviceTest];
```

### API Reference

* [`startRecordingDeviceTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest)
* [`stopRecordingDeviceTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest)
* [`startPlaybackDeviceTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)

## Considerations

If the input device fails to initialize, check the error message in [Developer Center](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Constants/AgoraErrorCode.html).
