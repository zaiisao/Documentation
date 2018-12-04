
---
title: Set the Stereo/High-quality Audio Profile
description: How to set high-quality audio on Windows
platform: Windows
updatedAt: Fri Nov 23 2018 08:44:08 GMT+0000 (UTC)
---
# Set the Stereo/High-quality Audio Profile
## Feature Description 

A high-fidelity audio is essential for professional audio scenarios, such as for podcasts and singing competitions. For example, podcasts require stereo and high-fidelity audio. The high-fidelity audio refers to an audio profile with a 48-KHz sampling rate and a 192-Kbps bitrate. 


## Implementation
Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Video/windows_video.md) for more information.

Agora SDK provides the [setAudioProfile](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e) method for developers to set appropriate audio profiles according to the scenarios. This method has two parameters:

  - `profile` sets the sampling rate, bitrate, encode mode, and the number of channels.
  - `scenario` sets the audio application scenario, for example entertainment, education, and live gaming. The SDK optimizes the audio fluency, noise control, and the audio quality based on the scenarios.

```c++
RtcEngineParameters rep(*lpAgoraEngine);
// Set the stereo and high-quality audio profile
int nRet = rep.setAudioProfile(AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, AUDIO_SCENARIO_DEFAULT);
```

Besides the stereo and high-quality audio quality, the sample code shows the frequently used parameter settings for your reference.

 ```C++
// FM high-quality
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_SHOWROOM);

// Gaming
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_SPEECH_STANDARD, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_GAMING);

// Entertainment
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_STANDARD, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);

// KTV
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_HIGH_QUALITY, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);
```

### API Reference

- [setAudioProfile](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e)

## Considerations

- Call this method before joining the channel.
- The `scenario` parameter takes effect only when the channel profile is live broadcast.
