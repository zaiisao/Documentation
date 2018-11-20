
---
title: Set the Stereo/High-quality Audio Profile
description: How to set high-quality audio on Windows
platform: Windows
updatedAt: Tue Nov 20 2018 07:05:07 GMT+0000 (UTC)
---
# Set the Stereo/High-quality Audio Profile
## Feature Description 

In some professional scenarios, the audio quality is essential for the user experience. For example, podcast applications require stereo and high-quality audio. The so-called high-quality audio refers to the audio profile of 48 KHz sampling rate and 192 Kbps bitrate, which suits the high-fidelity musical scenarios, such as online radio and singing competitions.

## Implementation

- Agora SDK provides the [setAudioProfile](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e) method for developers to set appropriate audio profiles according to the scenarios. This method has two parameters:

  - `profile` sets the sampling rate, bitrate, encode mode, and the number of channels.
  - `scenario` sets the audio application scenario, for example entertainment, education, and live gaming. The SDK optimizes the audio fluency, noise control, and the audio quality based on the scenarios.

  ```c++
  RtcEngineParameters rep(*lpAgoraEngine);
    // Set the stereo and high-quality audio profile
    int nRet = rep.setAudioProfile(AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, AUDIO_SCENARIO_DEFAULT);
  ```

  > For more options, see [setAudioProfile](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e).

## Considerations

- Call this method before joining the channel.
- The `scenario` parameter takes effect only when the channel profile is live broadcast.
