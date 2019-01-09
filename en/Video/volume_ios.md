
---
title: Adjust the Volume
description: How to adjust volume on iOS
platform: iOS
updatedAt: Wed Jan 09 2019 21:17:44 GMT+0000 (UTC)
---
# Adjust the Volume
## Introduction

When using the Agora SDK, developers can adjust the recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume to 0.

## Implementation
Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/ios_video.md).

The Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```swift
// swift
// Sets the volume of the recording signal.
agoraKit.adjustRecordingSignalVolume(50)
// Sets the volume of the playback signal.
agoraKit.adjustPlaybackSignalVolume(50)
```

```objective-c
// objective-c
// Sets the volume of the recording signal.
[agoraKit adjustRecordingSignalVolume: 50];
// Sets the volume of the playback signal.
[agoraKit adjustPlaybackSignalVolume: 50];
```

### API Methods

- [adjustRecordingSignalVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustRecordingSignalVolume:)
- [adjustPlaybackSignalVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:)

## Considerations

- If the volume of the audio signal is set too high, noise may occur on some devices.
- The API methods have return values. If the method fails, the return value is < 0.
