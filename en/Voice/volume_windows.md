
---
title: Adjust the Volume
description: How to adjust volume
platform: Windows
updatedAt: Tue Dec 04 2018 17:39:26 GMT+0000 (UTC)
---
# Adjust the Volume
## Feature Description

When using the Agora SDK, developers can adjust the recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume to 0.

## Implementation
Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/windows_video.md) for more information.

### Adjust the Volume of the Device

On Windows, Agora recommends adjusting the recording and playback volume by setting the device's volume:

```c++
// Set the recording volume
int setRecordingDeviceVolume(int volume);
// Set the playback volume
int setPlaybackDeviceVolume(int volume);
```

The value range of `volume` is 0 to 255. 0 means muting the device, and 255 represents the maximum volume of the device.
The value of `volume` corresponds to the system volume on Windows, as shown in the following screenshot.

![](https://web-cdn.agora.io/docs-files/1542792457096)

### Adjust the Volume of the Audio Signal 

If the above methods do not meet your requirements, Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volume.
The volume value range is 0 to 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```c++
RtcEngineParameters rep(*lpAgoraEngine);

// Set the volume of the recording audio signal to 200
int ret = rep.adjustRecordingSignalVolume(200);

// Set the volume of the playback audio signal to 200
int ret = rep.adjustPlaybackSignalVolume(200);
```

### API References

- [setRecordingDeviceVolume](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac24424e86ded2727a532df739ebf8086)
- [setPlaybackDeviceVolume](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac14a1238e83303abed2f36e02fcc9366)
- [adjustRecordingSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#aa9e9b5ae052022fe2e81232b9e6e7290)
- [adjustPlaybackSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8bed09e12b8e2d9934aafad50b77d364)

## Considerations

- If the volume of the audio signal is set too high, unnatural sounds may occur on some devices.
- The API methods have return values. If the method fails, the return value is < 0.

