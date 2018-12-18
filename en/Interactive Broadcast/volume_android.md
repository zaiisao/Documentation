
---
title: Adjust the Volume
description: How to adjust volume for Android
platform: Android
updatedAt: Tue Dec 18 2018 02:58:15 GMT+0000 (UTC)
---
# Adjust the Volume
## Introduction

When using the Agora SDK, developers can adjust the recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume to 0.

## Implementation
Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/android_video.md).

The Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```java
int volume = 200;
// Sets the volume of the recording signal.
rtcEngine.adjustRecordingSignalVolume(volume);
// Sets the volume of the playback signal.
rtcEngine.adjustPlaybackSignalVolume(volume);
```

### API Methods

- [adjustRecordingSignalVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af3747f72256eb683feadbca2b742bd05)
- [adjustPlaybackSignalVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af7d7f10fc96db2febb9c2590891d071b)

## Considerations

- If the volume of the audio signal is set too high, noise may occur on some devices.
- The API methods have return values. If the method fails, the return value is < 0.

