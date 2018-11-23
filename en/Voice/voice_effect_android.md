
---
title: Adjust the Pitch and Tone
description: How to adjust the voice effect on Android
platform: Android
updatedAt: Fri Nov 23 2018 08:54:57 GMT+0000 (UTC)
---
# Adjust the Pitch and Tone
## Feature Description 
In social and entertainment scenarios, the users often need various voice effects for more interesting and interacrtive experiences. Agora provides a series of methods for developer to flexibly change the users' voice, including adjusting the pitch, setting the equalization and reverberation modes.

## Implementation
Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/android_video.md) for more information.

The sample code shows how to change the original voice to Hulk's voice.

```java
// Set the pitch. The value range is [0.5, 2.0]. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
double pitch = 0.5;
rtcEngine.setLocalVoicePitch(pitch);

// Set the local voice equalization
// The first parameter sets the band frequency. The value range is [0, 9], each value representing the center frequency of the band: [31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, 16k] Hz
// The second parameter set the gain of each band. The value range is [-15,15], in dB, and the default is 0.
rtcEngine.setLocalVoiceEqualization(0, -15);
rtcEngine.setLocalVoiceEqualization(1, 3);
rtcEngine.setLocalVoiceEqualization(2, -9);
rtcEngine.setLocalVoiceEqualization(3, -8);
rtcEngine.setLocalVoiceEqualization(4, -6);
rtcEngine.setLocalVoiceEqualization(5, -4);
rtcEngine.setLocalVoiceEqualization(6, -3);
rtcEngine.setLocalVoiceEqualization(7, -2);
rtcEngine.setLocalVoiceEqualization(8, -1);
rtcEngine.setLocalVoiceEqualization(9, 1);

// The level of the dry signal in dB. The value ranges between -20 and 10.
rtcEngine.setLocalVoiceReverb(AudioConst.REVERB_DRY_LEVEL, 10);

// The level of the early reflection signal (wet signal) in dB. The value ranges between -20 and 10.
rtcEngine.setLocalVoiceReverb(AudioConst.REVERB_WET_LEVEL, 7);

// The room size of the reverberation. A larger romm size means a stronger reverberation. The value ranges between 0 and 100.
rtcEngine.setLocalVoiceReverb(AudioConst.REVERB_ROOM_SIZE, 6);

// The length of the initial delay of the wet signal (ms). The value range between 0 and 200.
rtcEngine.setLocalVoiceReverb(AudioConst.REVERB_WET_DELAY, 124);

// The reverberation strength. The value ranges between 0 and 100. The higher the value, the stronger the reverberation.
rtcEngine.setLocalVoiceReverb(AudioConst.REVERB_STRENGTH, 78);
```

### API References

- [setLocalVoicePitch](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a41b525f9cbf2911594bcda9b20a728c9)
- [setLocalVoiceEqualization](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e3aa79f0d6d8f2ea81907543506d960)
- [setLocalVoiceReverb](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4afc32ba68e997e90ba3f128317827fa)

## Considerations
The above methods have return values. If the API fails, the return is < 0.
