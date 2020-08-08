
---
title: Set the Voice Changer and Reverberation Effects
description: How to adjust the voice effect on Android
platform: Android
updatedAt: Tue Dec 10 2019 04:20:29 GMT+0800 (CST)
---
# Set the Voice Changer and Reverberation Effects
## Introduction 
## Introduction

In social and entertainment scenarios, users often want various voice enhancements or voice effects to improve their interactive experiences. For example, in chat rooms, a user can select a voice effect to add a virtual stereo effect to their voice.
To accomplish this, Agora RTC SDK provides preset voice effects. You can also dynamically change the users' voices, such as adjusting the pitch, setting the equalization, and reverberation modes. Try out the preset voice effects on the [online Demo](https://www.agora.io/en/audio-demo) provided by Agora. 

## Implementation

<% 
if (os == "android" || os == "windows") { %>
Before proceeding, ensure that you implement a basic call or the interactive live streaming in your project. See [Start a Call](../../en/Audio%20Broadcast/start_call_%3C%=os%20%%3E.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_%3C%=os%20%%3E.md) for details.
<% }

if (os == "apple") { %>
Before proceeding, ensure that you implement a basic call or the interactive live streaming in your project. See the following documents:

- iOS: [Start a Call](../../en/Audio%20Broadcast/start_call_ios.md) or [Start the interactive live streaming](../../en/Audio%20Broadcast/start_live_ios.md)
- macOS: [Start a Call](../../en/Audio%20Broadcast/start_call_mac.md) or [Start the interactive live streaming](../../en/Audio%20Broadcast/start_live_mac.md)
<% }
%>

### Use preset voice effects

The SDK provides voice enhancement and voice effects for different scenarios, as follows:

<table>
  <tr>
    <th colspan="2">Category</th>
    <th>Scenario</th>
  </tr>
  <tr>
    <td rowspan="2">Voice enhancement</td>
    <td>Chat enhancement</td>
    <td>Audio and video scenarios focusing on a user’s speaking voice:<li>Blind date</li><li>Emotional radio</li><li>Co-host audio streaming</li><li>Voice-only PK Hosting</li><li>Gaming chatroom</li></td>
  </tr>
  <tr>
    <td>Timbre transformation</td>
    <td>Audio and video scenarios focusing on a user’s speaking voice or singing voice:<li>Co-host audio streaming</li><li>Voice PK hosting</li><li>Gaming chatroom</li><li>Blind date</li><li>Online KTV</li><li>FM radio</li></td>
  </tr>
  <tr>
    <td rowspan="3">Voice effect</td>
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


<% 
if (os == "android" || os == "windows") { %>
<div class="alert note"><li>Before calling the method, you need to set the <tt>profile</tt> parameter of <tt>setAudioProfile</tt> to <tt>AUDIO_PROFILE_MUSIC_HIGH_QUALITY(4)</tt> or <tt>AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO(5)</tt>, and to set <tt>scenario</tt> parameter to <tt>AUDIO_SCENARIO_GAME_STREAMING(3)</tt>.</li><li>The voice effects preset in <tt>setLocalVoiceChanger</tt> and <tt>setLocalVoiceReverbPreset</tt> are mutually exclusive. The method called later overrides the one called earlier.</li> </div>
<% }

if (os == "apple") { %>
<div class="alert note"><li>Before calling the method, you need to set the <tt>profile</tt> parameter of <tt>setAudioProfile</tt> to <tt>AgoraAudioProfileMusicHighQuality(4)</tt> or <tt>AgoraAudioProfileMusicHighQualityStereo(5)</tt>, and to set <tt>scenario</tt> parameter to <tt>AgoraAudioScenarioGameStreaming(3)</tt>.</li><li>The voice effects preset in <tt>setLocalVoiceChanger</tt> and <tt>setLocalVoiceReverbPreset</tt> are mutually exclusive. The method called later overrides the one called earlier.</li> </div>
<% }
%>

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/start_live_android.md).

### Use a preset voice changer option and reverberation effect

You can use one of the following preset voice changer options by calling the `setLocalVoiceChanger` method:

- An old man's voice.
- A little boy's voice.
- A little girl's voice.
- Zhu Bajie's voice (Zhu Bajie is a character from Journey to the West who has a voice like a growling bear).
- Ethereal vocal effects.
- Hulk's voice.

```java
// Sets to an old man's voice.
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_OLDMAN);

// Sets to the user's original voice.
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_OFF);
```

You can use one of the following preset reverberation effects by calling the `setLocalVoiceReverbPreset` method:

- Pop music
- R&B
- Rock music
- Hip-hop
- Pop concert
- Karaoke
- Recording studio

```java
// Sets the preset reverberation effect to pop music.
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_POPULAR);

// Turns off reverberation.
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_OFF);
```

### Customize the voice effects

You can also customize the voice effects by adjusting the voice pitch, equalization, and reverberation settings.

The following sample code shows how to change from the original voice to Hulk's voice.

```java
// Sets the pitch. The value ranges between 0.5 and 2.0. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
double pitch = 0.5;
rtcEngine.setLocalVoicePitch(pitch);

// Sets the local voice equalization.
// The first parameter sets the band frequency. The value ranges between 0 and 9. Each value represents the center frequency of the band: 31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, and 16k Hz
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

### API Reference

- [`setLocalVoiceChanger`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ade6883c7878b7a596d5b2563462597dd)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a10dd25bc8e129512cd6727133b7fc42f)
- [`setLocalVoicePitch`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a41b525f9cbf2911594bcda9b20a728c9)
- [`setLocalVoiceEqualization`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e3aa79f0d6d8f2ea81907543506d960)
- [`setLocalVoiceReverb`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4afc32ba68e997e90ba3f128317827fa)

## Sample Code

Agora provides an open source sample code that implements voice changer and reverberation. You can go to the [Chatroom GitHub Repo](https://github.com/AgoraIO-Usecase/Chatroom/tree/master/Android) to download it.

## Considerations
The API methods have return values. If the method call fails, the return value is < 0.
