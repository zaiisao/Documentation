
---
title: Set the Voice Beautifier and Effects
description: How to adjust the voice effect on Android
platform: Android
updatedAt: Tue Aug 11 2020 10:14:01 GMT+0800 (CST)
---
# Set the Voice Beautifier and Effects
## Introduction

In social and entertainment scenarios, users often want various voice effects to improve their interactive experiences. For example, in chat rooms, a user can select a voice effect to add a virtual stereo effect to their voice.
To accomplish this, Agora RTC SDK provides preset voice effects. You can also dynamically change the users' voices, such as adjusting the pitch, setting the equalization, and reverberation modes. Try out the preset voice effects on the [online Demo](https://www.agora.io/en/audio-demo) provided by Agora. 

## Implementation


Before proceeding, ensure that you implement a basic call or the live interactive streaming in your project. See [Start a Call](../../en/Video/start_call_android.md) or [Start Live Interactive Streaming](../../en/Video/start_live_android.md) for details.


### Use preset voice effects

The SDK provides voice beautifier and voice effects for different scenarios, as follows:

<table>
  <tr>
    <th colspan="2">Category</th>
    <th>Scenario</th>
  </tr>
  <tr>
    <td rowspan="2">Voice beautifier</td>
    <td>Chat beautifier</td>
    <td>Audio and video scenarios focusing on a user’s speaking voice:<li>Blind date</li><li>Emotional radio</li><li>Co-host audio streaming</li><li>Voice-only PK Hosting</li><li>Gaming chatroom</li></td>
  </tr>
  <tr>
    <td>Timbre transformation</td>
    <td>Audio and video scenarios focusing on a user’s speaking voice or singing voice:<li>Co-host audio streaming</li><li>Voice PK hosting</li><li>Gaming chatroom</li><li>Blind date</li><li>Online KTV</li><li>FM radio</li></td>
  </tr>
  <tr>
    <td rowspan="3">Audio effect</td>
    <td>Voice changer effect</td>
    <td>Audio and video scenarios focusing on a user’s speaking voice:<li>Co-host audio streaming</li><li>Voice-only PK Hosting</li><li>Gaming chatroom</li></td>
  </tr>
  <tr>
    <td>Style transformation</td>
    <td>Audio and video scenarios focusing on a user’s singing voice:<li>Online KTV</li><li>Music radio</li><li>Live-streaming showroom</li></td>
  </tr>
  <tr>
    <td>Room acoustics</td>
    <td>Audio and video scenarios focusing on a user’s speaking voice or singing voice:<li>Co-host audio streaming</li><li>Voice PK hosting</li><li>Gaming chatroom</li><li>Blind date</li><li>Online KTV</li><li>FM radio</li></td>
  </tr>
 </table>

You can use the preset voice effects by calling `setLocalVoiceChanger` or `setLocalVoiceReverbPreset`.



<div class="alert note"><li>Before calling the method, you need to set the <tt>profile</tt> parameter of <tt>setAudioProfile</tt> to <tt>AUDIO_PROFILE_MUSIC_HIGH_QUALITY(4)</tt> or <tt>AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO(5)</tt>, and to set <tt>scenario</tt> parameter to <tt>AUDIO_SCENARIO_GAME_STREAMING(3)</tt>.</li><li>The voice effects preset in <tt>setLocalVoiceChanger</tt> and <tt>setLocalVoiceReverbPreset</tt> are mutually exclusive. The method called later overrides the one called earlier.</li> </div>


#### Chat beautifier 

Chat beautifier refers to beautifying the characteristics of male or female voices without altering the original voice beyond recognition.

You can implement the chat beautifier by the enumerations in `setLocalVoiceChanger` as follows:

| Enumeration                          | Description                                                  |
| :----------------------------------- | :----------------------------------------------------------- |
| `GENERAL_BEAUTY_VOICE_MALE_MAGNETIC`   | (For male-sounding voice only) A more magnetic voice. Do not use for speakers with a female-sounding voice; otherwise, voice distortion occurs. |
| `GENERAL_BEAUTY_VOICE_FEMALE_FRESH`    | (For female-sounding voice only) A fresher voice. Do not use for speakers with a male-sounding voice; otherwise, voice distortion occurs. |
| `GENERAL_BEAUTY_VOICE_FEMALE_VITALITY` | (For female-sounding voice only) A more vital voice. Do not use for speakers with a male-sounding voice; otherwise, voice distortion occurs. |

```java
// Beautifies the local voice by making it sound more magnetic (for male only).
mRtcEngine.setLocalVoiceChanger(GENERAL_BEAUTY_VOICE_MALE_MAGNETIC);
// Disables voice effects.
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_OFF);
```

#### Timbre transformation 

Timbre transformation changes the timbre of a voice in a specific way. Users can choose the most appropriate effect for their voice.

You can implement the timbre transformation by the enumerations in `setLocalVoiceChanger` as follows:

| Enumeration             | Description              |
| :---------------------- | :----------------------- |
| `VOICE_BEAUTY_VIGOROUS`   | A more vigorous voice.   |
| `VOICE_BEAUTY_DEEP`       | A deeper voice.          |
| `VOICE_BEAUTY_MELLOW`     | A mellower voice.        |
| `VOICE_BEAUTY_FALSETTO`   | Falsetto.                |
| `VOICE_BEAUTY_FULL`       | A fuller voice.          |
| `VOICE_BEAUTY_CLEAR`      | A clearer voice.         |
| `VOICE_BEAUTY_RESOUNDING` | A more resounding voice. |
| `VOICE_BEAUTY_RINGING`    | A more ringing voice.    |

```java
// Beautifies the local voice by making it sound more vigorous.
mRtcEngine.setLocalVoiceChanger(VOICE_BEAUTY_VIGOROUS);
// Disables voice effects.
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_OFF);
```

#### Voice changer effect

Voice changer effects adjust the voice to make it sound different from the original.

You can implement the voice changer effect by the enumerations in `setLocalVoiceChanger` or `setLocalVoiceReverbPreset` as follows:

<table>
  <tr>
    <th>Method</th>
		<th>Enumeration</th>
    <th>Description</th>
  </tr>
  <tr>
    <td rowspan="5"><tt>setLocalVoiceChanger</tt></td>
		<td><tt>VOICE_CHANGER_OLDMAN</tt></td>
    <td>(For male-sounding voice only) Voice of an old man.</td>
  </tr>
  <tr>
    <td><tt>VOICE_CHANGER_BABYBOY</tt></td>
    <td>(For male-sounding voice only) Voice of a little boy.</td>
  </tr>
  <tr>
    <td><tt>VOICE_CHANGER_BABYGIRL</tt></td>
    <td>(For female-sounding voice only) Voice of a little girl.</td>
  </tr>
  <tr>
    <td><tt>VOICE_CHANGER_ZHUBAJIE</tt></td>
		<td>Voice of the Zhu Bajie, a character in <i>Journey to the West</i> who has a voice like that of a growling bear.</td>
  </tr>
  <tr>
    <td><tt>VOICE_CHANGER_HULK</tt></td>
    <td>Voice of the Hulk. </td>
  </tr>
	<tr>
    <td rowspan="2"><tt>setLocalVoiceReverbPreset</tt></td>
		<td><tt>AUDIO_REVERB_FX_UNCLE</tt></td>
    <td>(For male-sounding voice only) Voice of a middle-aged uncle.</td>
  </tr>
  <tr>
    <td><tt>AUDIO_REVERB_FX_SISTER</tt></td>
    <td>(For female-sounding voice only) Voice of a maiden.</td>
  </tr>
 </table>

```java
// Presets the local voice by making it sound like the Hulk.
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_HULK);
// Disables voice effects.
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_OFF);
```

```java
// Presets the local voice by making it sound like a middle-aged uncle (for male-sounding voice only).
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_FX_UNCLE);
// Disables voice effects.
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_OFF);
```

#### Style transformation 

A style transformation makes singing more harmonious for specific styles of songs.

You can implement the style transformation by the enumerations in `setLocalVoiceReverbPreset` as follows:

<div class="alert note">To implement better voice effects, Agora recommends enumerations with <tt>AUDIO_REVERB_FX</tt> as the prefix.</div>

| Enumeration             | Description                                                 |
| :---------------------- | :---------------------------------------------------------- |
| `AUDIO_REVERB_FX_POPULAR` | The reverberation style typical of pop music.               |
| `AUDIO_REVERB_POPULAR`    | The reverberation style typical of pop music (old version). |
| `AUDIO_REVERB_FX_RNB`     | The reverberation style typical of R&B music.               |
| `AUDIO_REVERB_RNB`        | The reverberation style typical of R&B music (old version). |
| `AUDIO_REVERB_ROCK`       | The reverberation style typical of rock music.              |
| `AUDIO_REVERB_HIPHOP`     | The reverberation style typical of hip-hop music.           |

```java
// Presets the local voice to the style of pop music.
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_FX_POPULAR);
// Disables voice effects.
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_OFF);
```

#### Room acoustics 

Room acoustics refers to the spatial dimension added to a user’s voice for making the voice seem to come from a specific type of enclosed place.

You can implement the space construction by the enumerations in `setLocalVoiceChanger` or `setLocalVoiceReverbPreset` as follows:

<div class="alert note"><li>To implement better voice effects, Agora recommends enumerations with <tt>AUDIO_REVERB_FX</tt> as the prefix.</li><li>To achieve better virtual stereo reverberation, Agora recommends setting the <tt>profile</tt> parameter of <tt>setAudioProfile</tt> as <tt>AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO(5).</tt></li></div>

<table>
  <tr>
    <th>Method</th>
		<th>Enumeration</th>
    <th>Description</th>
  </tr>
  <tr>
    <td rowspan="2"><tt>setLocalVoiceChanger</tt></td>
		<td><tt>VOICE_BEAUTY_SPACIAL</tt></td>
    <td>A more spatially resonant voice.</td>
  </tr>
  <tr>
    <td><tt>VOICE_CHANGER_ETHEREAL</tt></td>
    <td>An ethereal voice</td>
  </tr>
  <tr>
		<td rowspan="8"><tt>setLocalVoiceReverbPreset</tt></td>
    <td><tt>AUDIO_REVERB_FX_VOCAL_CONCERT</tt></td>
    <td>The reverberation style typical of a concert hall.</td>
  </tr>
	<tr>
		<td><tt>AUDIO_REVERB_VOCAL_CONCERT</tt></td>
    <td>The reverberation style typical of a concert hall (old version).</td>
  </tr>
	<tr>
		<td><tt>AUDIO_REVERB_FX_KTV</tt></td>
    <td>The reverberation style typical of a KTV venue.</td>
  </tr>
  <tr>
    <td><tt>AUDIO_REVERB_KTV</tt></td>
    <td>The reverberation style typical of a KTV venue (old version).</td>
  </tr>
	<tr>
		<td><tt>AUDIO_REVERB_FX_STUDIO</tt></td>
    <td>The reverberation style typical of a recording studio.</td>
  </tr>
  <tr>
    <td><tt>AUDIO_REVERB_STUDIO</tt></td>
    <td>The reverberation style typical of a recording studio (old version). </td>
  </tr>
  <tr>
    <td><tt>AUDIO_REVERB_FX_PHONOGRAPH</tt></td>
    <td>The reverberation style typical of a vintage phonograph.</td>
  </tr>
	<tr>
		<td><tt>AUDIO_VIRTUAL_STEREO</tt></td>
		<td>A reverberation style that adds a virtual stereo effect. The virtual stereo is an effect that renders the monophonic audio as stereo.</td>
  </tr>
 </table>

```java
// Presets the local voice by making it sound more spatially resonant.
mRtcEngine.setLocalVoiceChanger(VOICE_BEAUTY_SPACIAL);
// Disables voice effects.
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_OFF);
```

```java
// Presets the local voice by making it sound like it is coming from a concert hall.
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_FX_VOCAL_CONCERT);
// Disables voice effects.
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_OFF);
```

### Customize voice effects 

If the preset effect does not meet your requirement, you can also customize the voice effects by adjusting the voice pitch, equalization, and reverberation settings through `setLocalVoicePitch`, `setLocalVoiceEqualization`, and `setLocalVoiceReverb`.

The following sample code shows how to change the original user's voice to the Hulk's voice.

```java
// Sets the pitch. The value ranges between 0.5 and 2.0. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
double pitch = 0.5;
rtcEngine.setLocalVoicePitch(pitch);
 
// Sets the local voice equalization.
// The first parameter sets the band frequency. The value ranges between 0 and 9. Each value represents the center frequency of the band: 31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, and 16k Hz.
// The second parameter sets the gain of each band. The value ranges between -15 and 15 dB. The default value is 0.
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
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_DRY_LEVEL, 10);
 
// The level of the early reflection signal (wet signal) in dB. The value ranges between -20 and 10.
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_WET_LEVEL, 7);
 
// The room size of the reverberation. A larger room size means a stronger reverberation. The value ranges between 0 and 100.
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_ROOM_SIZE, 6);
 
// The length of the initial delay of the wet signal (ms). The value ranges between 0 and 200.
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_WET_DELAY, 124);
 
// The reverberation strength. The value ranges between 0 and 100. The higher the value, the stronger the reverberation.
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_STRENGTH, 78);
```

##  API reference

**Preset voice effects**
- [`setLocalVoiceChanger`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ade6883c7878b7a596d5b2563462597dd)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a10dd25bc8e129512cd6727133b7fc42f)

**Customized voice effects**
- [`setLocalVoicePitch`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a41b525f9cbf2911594bcda9b20a728c9)
- [`setLocalVoiceEqualization`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e3aa79f0d6d8f2ea81907543506d960)
- [`setLocalVoiceReverb`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4afc32ba68e997e90ba3f128317827fa)

## Considerations

- All methods mentioned in this article work best with the human voice only. Agora does not recommend using them for audio containing both music and voice.
- Agora recommends not using the methods for presetting voice effects and for customizing voice effects together, as undefined behaviors may result. If you want to use preset voice effects together with methods that customize voice effects, call the preset methods before the customization methods, or the method called later overrides the one called earlier.
