
---
title: Set the Stereo/High-quality Audio Profile
description: How to set high-quality audio
platform: Android
updatedAt: Fri Nov 23 2018 08:42:02 GMT+0000 (UTC)
---
# Set the Stereo/High-quality Audio Profile
## Feature Description 
In some professional scenarios, the audio quality is essential for the user experience. For example, podcast applications require stereo and high-quality audio. The so-called high-quality audio refers to the audio profile of 48 KHz sampling rate and 192 Kbps bitrate, which suits the high-fidelity musical scenarios, such as online radio and singing competitions.

## Implementation
Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/android_video.md) for more information.

Agora SDK provides the [setAudioProfile](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a34175b5e04c88d9dc6608b1f38c0275d) method for developers to set appropriate audio profiles according to the scenarios. This method has two parameters:

- `profile` sets the sampling rate, bitrate, encode mode, and the number of channels.
- `scenario` sets the audio application scenario, for example entertainment, education, and live gaming. The SDK optimizes the audio fluency, noise control, and the audio quality based on the scenarios.

Besides the stereo and high-quality audio quality, the sample code shows the frequently used parameter settings for your reference.

```java
// FM high-quality
rtcEngine.setAudioProfile(Constants.AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, Constants.AUDIO_SCENARIO_SHOWROOM);

// Gaming
rtcEngine.setAudioProfile(Constants.AUDIO_PROFILE_SPEECH_STANDARD, Constants.AUDIO_SCENARIO_CHATROOM_GAMING);

// Entertainment
rtcEngine.setAudioProfile(Constants.AUDIO_PROFILE_MUSIC_STANDARD, Constants.AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);

// KTV
rtcEngine.setAudioProfile(Constants.AUDIO_AUDIO_PROFILE_MUSIC_HIGH_QUALITY, Constants.AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);
```

## Considerations

- Call this method before joining the channel.
- The `scenario`  parameter takes effect only when the channel profile is live broadcast.
