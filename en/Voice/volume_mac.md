
---
title: Adjust the Volume
description: How to adjust volume on macOS
platform: macOS
updatedAt: Tue Dec 04 2018 17:32:53 GMT+0000 (UTC)
---
# Adjust the Volume
## Feature Description

When using the Agora SDK, developers can adjust the recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume to 0.

## Implementation
Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Voice/mac_video.md) for more information.

### Adjust the Volume of the Device

On macOS, Agora recommends adjusting the recording and playback volumes by setting the device's volume:

```swift
// swift
// Sets the recording volume to 50.
agoraKit.setDeviceVolume(.audioRecording, volume: 50)
// Sets the playback volume to 50.
agoraKit.setDeviceVolume(.audioPlayout, volume: 50)
```

```objective-c
// objective-c
// Sets the recording volume to 50.
[agoraKit setDeviceVolume: AgoraMediaDeviceTypeAudioRecording, volume: 50]
// Sets the playback volume to 50.
[agoraKit setDeviceVolume: AgoraMediaDeviceTypeAudioPlayout, volume: 50];
```

The value of `volume` ranges between 0 and 255. 0 means muting the device, and 255 represents the maximum volume of the device.
The value of `volume` corresponds to the **Input Volume** or **Output Volume** on macOS, as shown in the following screenshot.

![](https://web-cdn.agora.io/docs-files/1542783111806)

### Adjust the Volume of the Audio Signal 

If the above methods do not meet your requirements, the Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```swift
// swift
// Sets the volume of the recording signal to 50.
agoraKit.adjustRecordingSignalVolume(50)
// Sets the volume of the playback signal to 50.
agoraKit.adjustPlaybackSignalVolume(50)
```

```objective-c
// objective-c
// Sets the volume of the recording signal to 50.
[agoraKit adjustRecordingSignalVolume: 50];
// Sets the volume of the playback signal to 50.
[agoraKit adjustPlaybackSignalVolume: 50];
```

### API Methods

- [setDeviceVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setDeviceVolume:volume:)
- [adjustRecordingSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustRecordingSignalVolume:)
- [adjustPlaybackSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:)

## Considerations

- If the volume of the audio signal is set too high, noise may occur on some devices.
- The API methods have return values. If the method fails, the return value is < 0.

