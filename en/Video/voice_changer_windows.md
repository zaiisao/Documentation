
---
title: Set the Voice Changer and Reverberation Effects
description: How to adjust pitch and tone on Windows
platform: Windows
updatedAt: Wed May 27 2020 11:13:29 GMT+0800 (CST)
---
# Set the Voice Changer and Reverberation Effects
## Introduction 

In social and entertainment scenarios, users often need various voice effects to enhance an interactive experience. To accomplish this, Agora provides multiple preset voice changers and reverberation effects. You can also dynamically change the users' voice, such as adjusting the pitch and setting the equalization and reverberation modes.

## Implementation

Before proceeding, ensure that you implement a basic call or live broadcast in your project. See [Start a Call](../../en/Video/start_call_windows.md) or [Start a Live Broadcast](../../en/Video/start_live_windows.md) for details.

### Preset voice effects

#### Voice enhancement

You can use one of the following preset voice changer options by calling `setLocalVoiceChanger`:

| Voice effect             | Description                                                  | Scenario          |
| :----------------------- | :----------------------------------------------------------- | :---------------- |
| VOICE_CHANGER_XXX        | Changes the local voice to an old man, a little boy, or the Hulk. | Voice talk        |
| VOICE_BEAUTY_XXX         | Beautifies the local voice by making it sound more vigorous, resounding, or adding spatial resonance. | <li>Voice talk<li>Singing</li> |
| GENERAL_VOICE_BEAUTY_XXX | Adds gender-based beautification effects to the local voice:<li>For a male voice: Adds magnetism to the voice.<li>For a female voice: Adds freshness or vitality to the voice.</li> | Voice talk        |

See details in the following table:

| Enumeration                          | Description                                                  |
| :----------------------------------- | :----------------------------------------------------------- |
| VOICE_CHANGER_OFF                    | Turn off the local voice changer and revert to the original voice. |
| VOICE_CHANGER_OLDMAN                 | The voice of an old man.                                     |
| VOICE_CHANGER_BABYBOY                | The voice of a little boy.                                   |
| VOICE_CHANGER_BABYGIRL               | The voice of a little girl.                                  |
| VOICE_CHANGER_ZHUBAJIE               | The voice of Zhu Bajie, a character in *Journey to the West* who has a voice like that of a growling bear. |
| VOICE_CHANGER_ETHEREAL               | The ethereal voice.                                          |
| VOICE_CHANGER_HULK                   | The voice of Hulk.                                           |
| VOICE_BEAUTY_VIGOROUS                | A more vigorous voice.                                       |
| VOICE_BEAUTY_DEEP                    | A deeper voice.                                              |
| VOICE_BEAUTY_MELLOW                  | A mellower voice.                                            |
| VOICE_BEAUTY_FALSETTO                | Falsetto.                                                    |
| VOICE_BEAUTY_FULL                    | A fuller voice.                                              |
| VOICE_BEAUTY_CLEAR                   | A clearer voice.                                             |
| VOICE_BEAUTY_RESOUNDING              | A more resounding voice.                                     |
| VOICE_BEAUTY_RINGING                 | A more ringing voice.                                        |
| VOICE_BEAUTY_SPACIAL                 | A more spatially resonant voice.                             |
| GENERAL_BEAUTY_VOICE_MALE_MAGNETIC   | (For male only) A more magnetic voice. Do not use it for speakers with a female-sounding voice; otherwise, voice distortion occurs. |
| GENERAL_BEAUTY_VOICE_FEMALE_FRESH    | (For female only) A fresher voice. Do not use it for speakers with a male-sounding voice; otherwise, voice distortion occurs. |
| GENERAL_BEAUTY_VOICE_FEMALE_VITALITY | (For female only) A more vital voice. Do not use it for speakers with a male-sounding voice; otherwise, voice distortion occurs. |

<div class="alert warning">Enumerations that begin with <tt>GENERAL_BEAUTY_VOICE</tt> add gender-based beautification effect to the local voice.<p>Gender-based beautification effect works well only when applied to voices with the following characteristics:<p><li>Suitable for male-sounding voices: <tt>GENERAL_BEAUTY_VOICE_MALE_MAGNETIC</tt>.<li>Suitable for female-sounding voices: <tt>GENERAL_BEAUTY_VOICE_FEMALE_FRESH</tt> or <tt>GENERAL_BEAUTY_VOICE_FEMALE_VITALITY</tt>.</li></p><p>Using an unsuitable effect can lead to voice distortion.</li></div>

<div class="alert note">Do not use this method with <tt>setLocalVoiceReverbPreset</tt>, because the method called later overrides the one called earlier.</div>

```c++
VOICE_CHANGER_PRESET voiceChanger;
// Beautifies the local voice by making it sound more vigorous.
voiceChanger = VOICE_BEAUTY_VIGOROUS;
rtcEngine.setLocalVoiceChanger(voiceChanger);
// Disables voice enhancement.
voiceChanger = VOICE_CHANGER_OFF;
rtcEngine.setLocalVoiceChanger(voiceChanger);
```

#### Voice reverberation

You can use one of the following preset voice reverberation options by calling `setLocalVoiceReverbPreset`:

| Enumeration                   | Description                                                  |
| :---------------------------- | :----------------------------------------------------------- |
| AUDIO_REVERB_OFF              | Turn off local voice reverberation and revert to the original voice. |
| AUDIO_REVERB_POPULAR          | The reverberation style typical of popular music.            |
| AUDIO_REVERB_RNB              | The reverberation style typical of R&B music.                |
| AUDIO_REVERB_ROCK             | The reverberation style typical of rock music.               |
| AUDIO_REVERB_HIPHOP           | The reverberation style typical of hip-hop music.            |
| AUDIO_REVERB_VOCAL_CONCERT    | The reverberation style typical of a concert hall.           |
| AUDIO_REVERB_KTV              | The reverberation style typical of a KTV venue.              |
| AUDIO_REVERB_STUDIO           | The reverberation style typical of a recording studio.       |
| AUDIO_REVERB_FX_KTV           | The reverberation style typical of a KTV venue (enhanced).   |
| AUDIO_REVERB_FX_VOCAL_CONCERT | The reverberation style typical of a concert hall (enhanced). |
| AUDIO_REVERB_FX_UNCLE         | The reverberation style typical of an uncle's voice.         |
| AUDIO_REVERB_FX_SISTER        | The reverberation style typical of a sister's voice.         |
| AUDIO_REVERB_FX_STUDIO        | The reverberation style typical of a recording studio (enhanced). |
| AUDIO_REVERB_FX_POPULAR       | The reverberation style typical of popular music (enhanced). |
| AUDIO_REVERB_FX_RNB           | The reverberation style typical of R&B music (enhanced).     |
| AUDIO_REVERB_FX_PHONOGRAPH    | The reverberation style typical of a vintage phonograph.     |
| AUDIO_VIRTUAL_STEREO          | A reverberation style that adds a virtual stereo effect. The virtual stereo is an effect that renders the monophonic audio as the stereo audio, so that all users in the channel can hear the stereo voice effect. To achieve better virtual stereo reverberation, Agora recommends setting `profile` in `setAudioProfile` as `AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO(5).` |

<div class="alert warning">When calling the <tt>setLocalVoiceReverbPreset</tt> method with an enumeration that begins with <tt>AUDIO_REVERB_FX</tt>, ensure that you set the <tt>profile</tt> parameter passed to the <tt>setAudioProfile</tt> as <tt>AUDIO_PROFILE_MUSIC_HIGH_QUALITY(4)</tt> or <tt>AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO(5)</tt>. Failure to do so prevents the <tt>setLocalVoiceReverbPreset</tt> method from setting the corresponding voice reverberation option.</div>

<div class="alert note">Do not use this method with <tt>setLocalVoiceChanger</tt>, because the method called later overrides the one called earlier.</div>

```c++
AUDIO_REVERB_PRESET reverbPreset;
// Sets the reverberation style typical of a concert hall (enhanced).
reverbPreset = AUDIO_REVERB_FX_VOCAL_CONCERT;
rtcEngine.setLocalVoiceReverbPreset(reverbPreset);
// Disables the voice reverberation.
reverbPreset = AUDIO_REVERB_OFF;
rtcEngine.setLocalVoiceReverbPreset(reverbPreset);
```

### Customize the voice effects

You can also customize the voice effects by adjusting the voice pitch, equalization, and reverberation settings.

The following sample code shows how to change from the original voice to Hulk's voice.

```c++
// Sets the pitch. The value ranges between 0.5 and 2.0. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
int nRet = rtcEngine.setLocalVoicePitch(0.5);

// Sets the local voice equalization.
// The first parameter sets the band frequency. The value ranges between 0 and 9. Each value represents the center frequency of the band: 31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, and 16k Hz
// The second parameter sets the gain of each band. The value ranges between -15 and 15 dB. The default value is 0.
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_31, -15);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_62, 3);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_125, -9);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_250, -8);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_500, -6);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_1K, -4);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_2K, -3);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_4K, -2);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_8K, -1);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_16K, 1);

// The level of the dry signal in dB. The value ranges between -20 and 10.
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_DRY_LEVEL, 10);

// The level of the early reflection signal (wet signal) in dB. The value ranges between -20 and 10.
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_WET_LEVEL, 7);

// The room size of the reverberation. A larger room size means a stronger reverberation. The value ranges between 0 and 100.
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_ROOM_SIZE, 6);

// The length of the initial delay of the wet signal (ms). The value ranges between 0 and 200.
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_WET_DELAY, 124);

// The reverberation strength. The value ranges between 0 and 100. The higher the value, the stronger the reverberation.
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_STRENGTH, 78);
```

### API reference

- [`setLocalVoiceChanger`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a99dc3d74202422436d40f6d7aa6e99dc)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a51a429a5a848b2ad591220aa6c24a898)
- [`setLocalVoicePitch`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a43616f919e0906279dff5648830ce31a)
- [`setLocalVoiceEqualization`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8c75994eb06ab26a1704715ec76e0189)
- [`setLocalVoiceReverb`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4d1d1309f97f3c430a1aa2d060bb7316)

## Considerations

- All methods mentioned in this guide work best with a human voice, and Agora recommends not using it for audio containing both music and a human voice.
- Agora recommends not using the methods for presetting voice effects and methods for customizing voice effects together. Failure to do so may result in undefined behaviors. If you want to use preset voice effects together with methods that customize voice effects, call the preset methods before the customization methods, or the method called later overrides the one called earlier.
- Agora recommends not using `setLocalVoiceChanger` and `setLocalVoiceReverbPreset` together, or the method called later overrides the one called earlier.
- To achieve a better voice effect quality, Agora recommends setting the `profile` parameter passed to the `setAudioProfile` as `AUDIO_PROFILE_MUSIC_HIGH_QUALITY(4)` or `AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO(5)` before calling `setLocalVoiceChanger` or `setLocalVoiceReverbPreset`.
