
---
title: Adjust the Volume
description: How to adjust volume for Android
platform: Android
updatedAt: Fri Nov 23 2018 07:26:51 GMT+0000 (UTC)
---
# Adjust the Volume
## Feature Description

When using the Agora SDK, developers can adjust the volumes of the captured audio and the playback, to satisfy customization requirements. For example, you can mute the remote audio by setting the volume to 0.

## Implementation
Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/android_video.md) for more information.

Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volume.
The volume value range is 0 to 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```java
int volume = 200;
// Set the volume of the recording audio signal.
rtcEngine.adjustRecordingSignalVolume(volume);
// Set the volume of the playback audio signal.
rtcEngine.adjustPlaybackSignalVolume(volume);
```

### API References

- [adjustRecordingSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af3747f72256eb683feadbca2b742bd05)
- [adjustPlaybackSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af7d7f10fc96db2febb9c2590891d071b)

## Considerations

- If the volume of the audio signal is set too high, unnatural sounds may occur on some devices.
- The above methods have return values. If the API fails, the return is < 0.

