
---
title: Adjust the Volume
description: How to adjust volume on macOS
platform: macOS
updatedAt: Fri Nov 23 2018 07:57:36 GMT+0000 (UTC)
---
# Adjust the Volume
## Feature Description

When using the Agora SDK, developers can adjust the audio recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume to 0.

## Implementation
Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/mac_video.md) for more information.

### Adjust the Volume of the Device

On macOS, Agora recommends adjusting the recording and playback volume by setting the device's volume:

```swift
// swift
// Set the recording volume to 50
agoraKit.setDeviceVolume(.audioRecording, volume: 50)
// Set the playback volume to 50
agoraKit.setDeviceVolume(.audioPlayout, volume: 50)
```

```objective-c
// objective-c
// Set the recording volume to 50
[agoraKit setDeviceVolume: AgoraMediaDeviceTypeAudioRecording, volume: 50]
// Set the playback volume to 50
[agoraKit setDeviceVolume: AgoraMediaDeviceTypeAudioPlayout, volume: 50];
```

The value range of `volume` is 0 to 255. 0 means muting the device, and 255 represents the maximum volume of the device.
The value of `volume` corresponds to the **Input Volume** or **Output Volume** on macOS, as shown in the  screenshot.

![](https://web-cdn.agora.io/docs-files/1542783111806)

### Adjust the Volume of the Audio Signal 

If the above methods do not meet your requirements, Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volume.
The volume value range is 0 to 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```swift
// swift
// Set the volume of the recording audio signal to 50
agoraKit.adjustRecordingSignalVolume(50)
// Set the volume of the playback audio signal to 50
agoraKit.adjustPlaybackSignalVolume(50)
```

```objective-c
// objective-c
// Set the volume of the recording audio signal to 50
[agoraKit adjustRecordingSignalVolume: 50];
// Set the volume of the playback audio signal to 50
[agoraKit adjustPlaybackSignalVolume: 50];
```

### API References

- [setDeviceVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setDeviceVolume:volume:)
- [adjustRecordingSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustRecordingSignalVolume:)
- [adjustPlaybackSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:)

## Considerations

- If the volume of the audio signal is set too high, unnatural sounds may occur on some devices.
- The above methods have return values. If the API fails, the return is < 0.

