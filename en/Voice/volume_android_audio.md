
---
title: Adjust the Volume
description: How to adjust volume for Android
platform: Android
updatedAt: Wed Jan 09 2019 21:44:28 GMT+0000 (UTC)
---
# Adjust the Volume
## Introduction

When using the Agora SDK, you can adjust the audio recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume as 0.

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Voice/android_audio.md).

The Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volumes.
The value of the volume ranges between 0 and 400. 100 (default) is the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```java
int volume = 200;
// Sets the volume of the recording signal.
rtcEngine.adjustRecordingSignalVolume(volume);
// Sets the volume of the playback signal.
rtcEngine.adjustPlaybackSignalVolume(volume);
```

### API Reference

- [`adjustRecordingSignalVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af3747f72256eb683feadbca2b742bd05)
- [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af7d7f10fc96db2febb9c2590891d071b)

## Considerations

- If the volume of the audio signal is set too high, noise may occur on some devices.
- The API methods have return values. If the method call fails, the return value is < 0.

