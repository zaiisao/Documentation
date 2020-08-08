
---
title: Set the Voice Enhancement and Effects
description: How to set voice effects on iOS and macOS
platform: iOS,macOS
updatedAt: Sat Aug 08 2020 16:12:56 GMT+0800 (CST)
---
# Set the Voice Enhancement and Effects
## Introduction

In social and entertainment scenarios, users often want various voice enhancements or voice effects to improve their interactive experiences. For example, in chat rooms, a user can select a voice effect to add a virtual stereo effect to their voice.
To accomplish this, Agora RTC SDK provides preset voice effects. You can also dynamically change the users' voices, such as adjusting the pitch, setting the equalization, and reverberation modes. Try out the preset voice effects on the [online Demo](https://www.agora.io/en/audio-demo) provided by Agora. 

## Implementation


Before proceeding, ensure that you implement a basic call or the live interactive streaming in your project. See the following documents:

- iOS: [Start a Call](../../en/Voice/start_call_ios.md) or [Start Live Interactive Streaming](../../en/Voice/start_live_ios.md)
- macOS: [Start a Call](../../en/Voice/start_call_mac.md) or [Start Live Interactive Streaming](../../en/Voice/start_live_mac.md)


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



<div class="alert note"><li>Before calling the method, you need to set the <tt>profile</tt> parameter of <tt>setAudioProfile</tt> to <tt>AgoraAudioProfileMusicHighQuality(4)</tt> or <tt>AgoraAudioProfileMusicHighQualityStereo(5)</tt>, and to set <tt>scenario</tt> parameter to <tt>AgoraAudioScenarioGameStreaming(3)</tt>.</li><li>The voice effects preset in <tt>setLocalVoiceChanger</tt> and <tt>setLocalVoiceReverbPreset</tt> are mutually exclusive. The method called later overrides the one called earlier.</li> </div>


#### Chat enhancement 

Chat enhancement refers to enhancing the characteristics of male or female voices without altering the original voice beyond recognition.

You can implement the chat enhancement by the enumerations in `setLocalVoiceChanger` as follows:

| Enumeration                          | Description                                                  |
| :----------------------------------- | :----------------------------------------------------------- |
| `AgoraAudioGeneralBeautyVoiceMaleMagnetic`   | (For male-sounding voice only) A more magnetic voice. Do not use for speakers with a female-sounding voice; otherwise, voice distortion occurs. |
| `AgoraAudioGeneralBeautyVoiceFemaleFresh`    | (For female-sounding voice only) A fresher voice. Do not use for speakers with a male-sounding voice; otherwise, voice distortion occurs. |
| `AgoraAudioGeneralBeautyVoiceFemaleVitality` | (For female-sounding voice only) A more vital voice. Do not use for speakers with a male-sounding voice; otherwise, voice distortion occurs. |

```swift
// swift
// Beautifies the local voice by making it sound more magnetic (for male only).
agoraKit.setLocalVoiceChanger(.generalBeautyVoiceMaleMagnetic)
// Disables voice effects.
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// Beautifies the local voice by making it sound more magnetic (for male only).
[self.agoraKit setLocalVoiceChanger: AgoraAudioGeneralBeautyVoiceMaleMagnetic];
// Disables voice effects.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

#### Timbre transformation 

Timbre transformation changes the timbre of a voice in a specific way. Users can choose the most appropriate effect for their voice.

You can implement the timbre transformation by the enumerations in `setLocalVoiceChanger` as follows:

| Enumeration             | Description              |
| :---------------------- | :----------------------- |
| `AgoraAudioVoiceBeautyVigorous`   | A more vigorous voice.   |
| `AgoraAudioVoiceBeautyDeep`       | A deeper voice.          |
| `AgoraAudioVoiceBeautyMellow`     | A mellower voice.        |
| `AgoraAudioVoiceBeautyFalsetto`   | Falsetto.                |
| `AgoraAudioVoiceBeautyFull`       | A fuller voice.          |
| `AgoraAudioVoiceBeautyClear`      | A clearer voice.         |
| `AgoraAudioVoiceBeautyResounding` | A more resounding voice. |
| `AgoraAudioVoiceBeautyRinging`    | A more ringing voice.    |

```swift
// swift
// Beautifies the local voice by making it sound more vigorous.
agoraKit.setLocalVoiceChanger(.voiceBeautyVigorous)
// Disables voice effects.
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// Beautifies the local voice by making it sound more vigorous.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceBeautyVigorous];
// Disables voice effects.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
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
		<td><tt>AgoraAudioVoiceChangerOldMan</tt></td>
    <td>(For male-sounding voice only) Voice of an old man.</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerBabyBoy</tt></td>
    <td>(For male-sounding voice only) Voice of a little boy.</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerBabyGirl</tt></td>
    <td>(For female-sounding voice only) Voice of a little girl.</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerZhuBaJie</tt></td>
		<td>Voice of the Zhu Bajie, a character in <i>Journey to the West</i> who has a voice like that of a growling bear.</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerHulk</tt></td>
    <td>Voice of the Hulk. </td>
  </tr>
	<tr>
    <td rowspan="2"><tt>setLocalVoiceReverbPreset</tt></td>
		<td><tt>AgoraAudioReverbPresetFxUncle</tt></td>
    <td>(For male-sounding voice only) Voice of a middle-aged uncle.</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioReverbPresetFxSister</tt></td>
    <td>(For female-sounding voice only) Voice of a maiden.</td>
  </tr>
 </table>

```swift
// swift
// Presets the local voice by making it sound like the Hulk.
agoraKit.setLocalVoiceChanger(.voiceChangerHulk)
// Disables voice effects.
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// Presets the local voice by making it sound like the Hulk.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerHulk];
// Disables voice effects.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

#### Style transformation 

A style transformation makes singing more harmonious for specific styles of songs.

You can implement the style transformation by the enumerations in `setLocalVoiceReverbPreset` as follows:

<div class="alert note">To implement better voice effects, Agora recommends enumerations with <tt>AgoraAudioReverbPresetFx</tt> as the prefix.</div>

| Enumeration             | Description                                                 |
| :---------------------- | :---------------------------------------------------------- |
| `AgoraAudioReverbPresetFxPopular` | The reverberation style typical of pop music.               |
| `AgoraAudioReverbPresetPopular`    | The reverberation style typical of pop music (old version). |
| `AgoraAudioReverbPresetFxRNB`     | The reverberation style typical of R&B music.               |
| `AgoraAudioReverbPresetRnB`        | The reverberation style typical of R&B music (old version). |
| `AgoraAudioReverbPresetRock`       | The reverberation style typical of rock music.              |
| `AgoraAudioReverbPresetHipHop`     | The reverberation style typical of hip-hop music.           |

```swift
// swift
// Presets the local voice to the style of pop music.
agoraKit.setLocalVoiceReverbPreset(.fxPopular)
// Disables voice effects.
agoraKit.setLocalVoiceReverbPreset(.off)
```

```objective-c
// objective-c
// Presets the local voice to the style of pop music.
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetFxPopular];
// Disables voice effects.
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetOff];
```

#### Room acoustics 

Room acoustics refers to the spatial dimension added to a user’s voice for making the voice seem to come from a specific type of enclosed place.

You can implement the space construction by the enumerations in `setLocalVoiceChanger` or `setLocalVoiceReverbPreset` as follows:

<div class="alert note"><li>To implement better voice effects, Agora recommends enumerations with <tt>AgoraAudioReverbPresetFx</tt> as the prefix.</li><li>To achieve better virtual stereo reverberation, Agora recommends setting the <tt>profile</tt> parameter of <tt>setAudioProfile</tt> as <tt>AgoraAudioProfileMusicHighQualityStereo(5).</tt></li></div>

<table>
  <tr>
    <th>Method</th>
		<th>Enumeration</th>
    <th>Description</th>
  </tr>
  <tr>
    <td rowspan="2"><tt>setLocalVoiceChanger</tt></td>
		<td><tt>AgoraAudioVoiceBeautySpacial</tt></td>
    <td>A more spatially resonant voice.</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerEthereal</tt></td>
    <td>An ethereal voice</td>
  </tr>
  <tr>
		<td rowspan="8"><tt>setLocalVoiceReverbPreset</tt></td>
    <td><tt>AgoraAudioReverbPresetFxVocalConcert</tt></td>
    <td>The reverberation style typical of a concert hall.</td>
  </tr>
	<tr>
		<td><tt>AgoraAudioReverbPresetVocalConcert</tt></td>
    <td>The reverberation style typical of a concert hall (old version).</td>
  </tr>
	<tr>
		<td><tt>AgoraAudioReverbPresetFxKTV</tt></td>
    <td>The reverberation style typical of a KTV venue.</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioReverbPresetKTV</tt></td>
    <td>The reverberation style typical of a KTV venue (old version).</td>
  </tr>
	<tr>
		<td><tt>AgoraAudioReverbPresetFxStudio</tt></td>
    <td>The reverberation style typical of a recording studio.</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioReverbPresetStudio</tt></td>
    <td>The reverberation style typical of a recording studio (old version). </td>
  </tr>
  <tr>
    <td><tt>AgoraAudioReverbPresetFxPhonograph</tt></td>
    <td>The reverberation style typical of a vintage phonograph.</td>
  </tr>
	<tr>
		<td><tt>AgoraAudioReverbPresetVirtualStereo</tt></td>
		<td>A reverberation style that adds a virtual stereo effect. The virtual stereo is an effect that renders the monophonic audio as stereo.</td>
  </tr>
 </table>

```swift
// swift
// Presets the local voice by making it sound more spatially resonant.
agoraKit.setLocalVoiceChanger(.voiceBeautySpacial)
// Disables voice effects.
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// Presets the local voice by making it sound more spatially resonant.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceBeautySpacial];
// Disables voice effects.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

### Customize voice effects 

If the preset effect does not meet your requirement, you can also customize the voice effects by adjusting the voice pitch, equalization, and reverberation settings through `setLocalVoicePitch`, `setLocalVoiceEqualization`, and `setLocalVoiceReverb`.

The following sample code shows how to change the original user's voice to the Hulk's voice.

```swift
// swift
// Sets the pitch. The value ranges between 0.5 and 2.0. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
agoraKit.setLocalVoicePitch(1)
 
// Sets the local voice equalization.
// The first parameter sets the band frequency. The value ranges between 0 and 9. Each value represents the center frequency of the band: 31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, and 16k Hz.
// The second parameter sets the gain of each band. The value ranges between -15 and 15 dB. The default value is 0.
agoraKit.setLocalVoiceEqualizationOf(.band31, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band62, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band125, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band250, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band500, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band1K, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band2K, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band4K, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band8K, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band16K, withGain: 0)
 
// The level of the dry signal in dB. The value ranges between -20 and 10.
agoraKit.setLocalVoiceReverbOf(.dryLevel, withValue: -1)
 
// The level of the early reflection signal (wet signal) in dB. The value ranges between -20 and 10.
agoraKit.setLocalVoiceReverbOf(.wetLevel, withValue: -7)
 
// The room size of the reverberation. A larger room size means a stronger reverberation. The value ranges between 0 and 100.
agoraKit.setLocalVoiceReverbOf(.roomSize, withValue: 57)
 
// The length of the initial delay of the wet signal (ms). The value ranges between 0 and 200.
agoraKit.setLocalVoiceReverbOf(.wetDelay, withValue: 135)
 
// The reverberation strength. The value ranges between 0 and 100. The higher the value, the stronger the reverberation.
agoraKit.setLocalVoiceReverbOf(.strength, withValue: 45)
```

```objective-c
// objective-c
// Sets the pitch. The value ranges between 0.5 and 2.0. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
[agoraKit setLocalVoicePitch: 1];
 
// Sets the local voice equalization.
// The first parameter sets the band frequency. The value ranges between 0 and 9. Each value represents the center frequency of the band: 31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, and 16k Hz.
// The second parameter sets the gain of each band. The value ranges between -15 and 15 dB. The default value is 0.
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand31 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand62 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand125 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand250 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand500 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand1K withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand2K withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand4K withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand8K withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand16K withGain: 0];
 
// The level of the dry signal in dB. The value ranges between -20 and 10.
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbDryLevel withValue: -1];
 
// The level of the early reflection signal (wet signal) in dB. The value ranges between -20 and 10.
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbWetLevel withValue: -7];
 
// The room size of the reverberation. A larger room size means a stronger reverberation. The value ranges between 0 and 100.
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbRoomSize withValue: 57];
 
// The length of the initial delay of the wet signal (ms). The value ranges between 0 and 200.
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbWetDelay withValue: 135];
 
// The reverberation strength. The value ranges between 0 and 100. The higher the value, the stronger the reverberation.
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbStrength withValue: 45];
```

##  API reference

**Preset voice effects**
- [`setLocalVoiceChanger`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)

**Customized voice effects**
- [`setLocalVoicePitch`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setLocalVoiceEqualizationOfBandFrequency`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceEqualizationOfBandFrequency:withGain:)
- [`setLocalVoiceReverbOfType`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbOfType:withValue:)

## Considerations

- All methods mentioned in this article work best with the human voice only. Agora does not recommend using them for audio containing both music and voice.
- Agora recommends not using the methods for presetting voice effects and for customizing voice effects together, as undefined behaviors may result. If you want to use preset voice effects together with methods that customize voice effects, call the preset methods before the customization methods, or the method called later overrides the one called earlier.
