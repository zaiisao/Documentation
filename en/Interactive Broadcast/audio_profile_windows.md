
---
title: Set the Stereo/High-fidelity Audio Profile
description: How to set high-quality audio on Windows
platform: Windows
updatedAt: Thu Aug 15 2019 04:09:17 GMT+0800 (CST)
---
# Set the Stereo/High-fidelity Audio Profile
## Introduction 

High-fidelity audio is essential for professional audio scenarios, such as for podcasts and singing competitions. For example, podcasts require stereo and high-fidelity audio. High-fidelity audio refers to an audio profile with a 48-KHz sampling rate and a 192-Kbps bitrate. 


## Implementation
Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/windows_video.md) for more information.

The Agora SDK provides the [setAudioProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e) method for developers to set appropriate audio profiles according to the scenarios. This method has two parameters:

  - `profile` sets the sampling rate, bitrate, encoding mode, and the number of channels.
  - `scenario` sets the audio application scenario. For example, entertainment, education, or live gaming. The SDK optimizes the noise control and audio quality based on the scenarios.

```c++
RtcEngineParameters rep(*lpAgoraEngine);
// Sets the stereo and high-fidelity audio profile
int nRet = rep.setAudioProfile(AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, AUDIO_SCENARIO_DEFAULT);
```

Besides the stereo and high-fidelity audio qualities, the following sample code shows the frequently used parameter settings for your reference.

 ```C++
// FM high-fidelity
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_SHOWROOM);

// Gaming
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_SPEECH_STANDARD, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_GAMING);

// Entertainment
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_STANDARD, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);

// KTV
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_HIGH_QUALITY, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);
```

### API Reference

- [`setAudioProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e)

## Considerations

- Call the [setAudioProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e) method before joining the channel.
- The `scenario` parameter takes effect only when the channel profile is live broadcast.
