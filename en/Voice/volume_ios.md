
---
title: Adjust the Volume
description: How to adjust volume on iOS
platform: iOS
updatedAt: Fri Nov 23 2018 07:50:39 GMT+0000 (UTC)
---
# Adjust the Volume
## Feature Description

When using the Agora SDK, developers can adjust the volumes of the captured audio and the playback, to satisfy customization requirements. For example, you can mute the remote audio by setting the volume to 0.

## Implementation

Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volume.
The volume value range is 0 to 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```swift
// swift
// Set the volume of the recording audio signal.
agoraKit.adjustRecordingSignalVolume(50)
// Set the volume of the playback audio signal.
agoraKit.adjustPlaybackSignalVolume(50)
```

```objective-c
// objective-c
// Set the volume of the recording audio signal.
[agoraKit adjustRecordingSignalVolume: 50];
// Set the volume of the playback audio signal.
[agoraKit adjustPlaybackSignalVolume: 50];
```

### API References

- [adjustRecordingSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustRecordingSignalVolume:)
- [adjustPlaybackSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:)

## Considerations

- If the volume of the audio signal is set too high, unnatural sounds may occur on some devices.
- The above methods have return values. If the API fails, the return is < 0.
