
---
title: Set the Voice Enhancement and Effects
description: How to adjust pitch and tone on Windows
platform: Unity
updatedAt: Fri Jun 19 2020 14:04:26 GMT+0800 (CST)
---
# Set the Voice Enhancement and Effects
## Introduction 

In social and entertainment scenarios, users often need various voice effects to enhance an interactive experience. To accomplish this, Agora provides multiple preset voice changers and reverberation effects. You can also dynamically change the users' voice, such as adjusting the pitch and setting the equalization and reverberation modes.

Agora provides an [online demo](https://www.agora.io/en/audio-demo) to try out the voice changer and reverberation effects.

## Implementation

Before proceeding, ensure that you implement a basic call or live broadcast in your project. See [Start a Call](../../en/Interactive%20Broadcast/start_call_unity.md) or [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_unity.md) for details.

### Use a preset voice changer and reverberation effect

You can use one of the following preset voice changer options by calling  `SetLocalVoiceChanger`:

- An old man's voice.
- A little boy's voice.
- A little girl's voice.
- Zhu Bajie's voice (Zhu Bajie is a character from Journey to the West who has a voice like a growling bear).
- Ethereal vocal effects.
- Hulk's voice.

**Sample code**

```c#
// Set the voice as a little boy's voice.
public int SetLocalVoiceChanger() 
{
  return mRtcEngine.SetLocalVoiceChanger(VOICE_CHANGER_PRESET.VOICE_CHANGER_BABYBOY);
}
```

You can use one of the following preset reverberation effects by calling `SetLocalVoiceReverbPreset`:

- Pop music
- R&B
- Rock music
- Hip-hop
- Pop concert
- Karaoke
- Recording studio

**Sample code**

```c#
// Set the reverberation effect as the pop music.
public int SetLocalVoiceReverbPreset() 
{
  return mRtcEngine.SetLocalVoiceReverbPreset(AUDIO_REVERB_PRESET.AUDIO_REVERB_POPULAR);
}
```

### Customize the voice effects

You can also customize the voice effects by adjusting the voice pitch, equalization, and reverberation settings.

**Sample code**
The following sample code shows how to change from the original voice to Hulk's voice.

```c#
// Sets the pitch. The value ranges between 0.5 and 2.0. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
public int SetLocalVoicePitch(double pitch) 
{
  return mRtcEngine.SetLocalVoicePitch(pitch);
}

// Sets the local voice equalization.
// The first parameter sets the band frequency. The value ranges between 0 and 9. Each value represents the center frequency of the band: 31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, and 16k Hz.
// The second parameter sets the gain of each band. The value ranges between -15 and 15 dB. The default value is 0.
public void SetLocalVoiceEqualization() 
{
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_31, -15);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_62, 3);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_125, -9);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_250, -8);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_500, -6);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_1K, -4);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_2K, -3);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_4K, -2);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_8K, -1);
  mRtcEngine.SetLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY.AUDIO_EQUALIZATION_BAND_16K, 1);
}
	
public void SetLocalVoiceReverb()
{
  // The level of the dry signal in dB. The value ranges between -20 and 10.
  mRtcEngine.SetLocalVoiceReverb(AUDIO_REVERB_TYPE.AUDIO_REVERB_DRY_LEVEL, 10);
  // The level of the early reflection signal (wet signal) in dB. The value ranges between -20 and 10.
  mRtcEngine.SetLocalVoiceReverb(AUDIO_REVERB_TYPE.AUDIO_REVERB_WET_LEVEL, 7);
  // The room size of the reverberation. A larger room size means a stronger reverberation. The value ranges between 0 and 100.
  mRtcEngine.SetLocalVoiceReverb(AUDIO_REVERB_TYPE.AUDIO_REVERB_ROOM_SIZE, 6);
  // The length of the initial delay of the wet signal (ms). The value ranges between 0 and 200.
  mRtcEngine.SetLocalVoiceReverb(AUDIO_REVERB_TYPE.AUDIO_REVERB_WET_DELAY, 124);
  // The reverberation strength. The value ranges between 0 and 100. The higher the value, the stronger the reverberation.
  mRtcEngine.SetLocalVoiceReverb(AUDIO_REVERB_TYPE.AUDIO_REVERB_STRENGTH, 78);
}
```

### API reference

- [`SetLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a6143c1720c020082f58b8bcf7b823fe1)
- [`SetLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a661775d82700681166589747262ef400)
- [`SetLocalVoicePitch`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#aa4b05f6b03d172520a887989be81b20e)
- [`SetLocalVoiceEqualization`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a9737e27e79e059e42e01a0e3b26e4212)
- [`SetLocalVoiceReverb`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#aab74b2ee8ab0e33b75667cf5bb7cc4bf)

## Considerations

The API methods have return values. If the method call fails, the return value is < 0.
