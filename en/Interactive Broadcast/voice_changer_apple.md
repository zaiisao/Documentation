
---
title: Set the Voice Enhancement and Effects
description: How to set voice effects on iOS and macOS
platform: iOS,macOS
updatedAt: Mon Jul 06 2020 09:46:37 GMT+0800 (CST)
---
# Set the Voice Enhancement and Effects
## Introduction 

In social and entertainment scenarios, users often need various voice effects to enhance an interactive experience. To accomplish this, Agora provides multiple preset voice changers and reverberation effects. You can also dynamically change the users' voice, such as adjusting the pitch and setting the equalization and reverberation modes.

Agora provides an [online demo](https://www.agora.io/en/audio-demo) to try out the voice changer and reverberation effects.

## Implementation

Before proceeding, ensure that you implement a basic call or live interactive streaming in your project. See the Quickstart Guides for details:

- iOS: [Start a Call](../../en/Interactive%20Broadcast/start_call_ios.md)/[Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_ios.md)
- macOS: [Start a Call](../../en/Interactive%20Broadcast/start_call_mac.md)/[Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_mac.md)

### Preset voice effects

#### Voice enhancement

You can use one of the following preset voice changer options by calling `setLocalVoiceChanger`:

| Voice effect             | Description                                                  | Scenario          |
| :----------------------- | :----------------------------------------------------------- | :---------------- |
| AgoraAudioVoiceChangerXXX        | Changes the local voice to an old man, a little boy, or the Hulk. | Voice talk        |
| AgoraAudioVoiceBeautyXXX         | Beautifies the local voice by making it sound more vigorous, resounding, or adding spatial resonance. | <li>Voice talk<li>Singing</li> |
| AgoraAudioGeneralBeautyVoiceXXX | Adds gender-based beautification effects to the local voice:<li>For a male voice: Adds magnetism to the voice.<li>For a female voice: Adds freshness or vitality to the voice.</li> | Voice talk        |

See details in the following table:

| Enumeration                          | Description                                                  |
| :----------------------------------- | :----------------------------------------------------------- |
| AgoraAudioVoiceChangerOldMan                 | The voice of an old man.                                     |
| AgoraAudioVoiceChangerBabyBoy                | The voice of a little boy.                                   |
| AgoraAudioVoiceChangerBabyGirl               | The voice of a little girl.                                  |
| AgoraAudioVoiceChangerZhuBaJie               | The voice of Zhu Bajie, a character in *Journey to the West* who has a voice like that of a growling bear. |
| AgoraAudioVoiceChangerEthereal               | The ethereal voice.                                          |
| AgoraAudioVoiceChangerHulk                   | The voice of Hulk.                                           |
| AgoraAudioVoiceBeautyVigorous                | A more vigorous voice.                                       |
| AgoraAudioVoiceBeautyDeep                    | A deeper voice.                                              |
| AgoraAudioVoiceBeautyMellow                  | A mellower voice.                                            |
| AgoraAudioVoiceBeautyFalsetto                | Falsetto.                                                    |
| AgoraAudioVoiceBeautyFull                    | A fuller voice.                                              |
| AgoraAudioVoiceBeautyClear                   | A clearer voice.                                             |
| AgoraAudioVoiceBeautyResounding              | A more resounding voice.                                     |
| AgoraAudioVoiceBeautyRinging                 | A more ringing voice.                                        |
| AgoraAudioVoiceBeautySpacial                 | A more spatially resonant voice.                             |
| AgoraAudioGeneralBeautyVoiceMaleMagnetic   | (For male only) A more magnetic voice. Do not use it for speakers with a female-sounding voice; otherwise, voice distortion occurs. |
| AgoraAudioGeneralBeautyVoiceFemaleFresh    | (For female only) A fresher voice. Do not use it for speakers with a male-sounding voice; otherwise, voice distortion occurs. |
| AgoraAudioGeneralBeautyVoiceFemaleVitality | (For female only) A more vital voice. Do not use it for speakers with a male-sounding voice; otherwise, voice distortion occurs. |

<div class="alert warning">Enumerations that begin with <tt>AgoraAudioGeneralBeautyVoice</tt> add gender-based beautification effect to the local voice.<p>Gender-based beautification effect works well only when applied to voices with the following characteristics:<p><li>Suitable for male-sounding voices: <tt>AgoraAudioGeneralBeautyVoiceMaleMagnetic</tt>.<li>Suitable for female-sounding voices: <tt>AgoraAudioGeneralBeautyVoiceFemaleFresh</tt> or <tt>AgoraAudioGeneralBeautyVoiceFemaleVitality</tt>.</li></p><p>Using an unsuitable effect can lead to voice distortion.</li></div>

<div class="alert note">Do not use this method with <tt>setLocalVoiceReverbPreset</tt>, because the method called later overrides the one called earlier.</div>

```swift
// swift
// Beautifies the local voice by making it sound more vigorous.
agoraKit.setLocalVoiceChanger(.voiceBeautyVigorous)
// Disables voice enhancement.
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// Beautifies the local voice by making it sound more vigorous.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceBeautyVigorous];
// Disables voice enhancement.
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

#### Voice reverberation effects

You can use one of the following preset voice reverberation options by calling `setLocalVoiceReverbPreset`:

| Enumeration                   | Description                                                  |
| :---------------------------- | :----------------------------------------------------------- |
| AgoraAudioReverbPresetPopular          | The reverberation style typical of popular music.            |
| AgoraAudioReverbPresetRnB              | The reverberation style typical of R&B music.                |
| AgoraAudioReverbPresetRock             | The reverberation style typical of rock music.               |
| AgoraAudioReverbPresetHipHop           | The reverberation style typical of hip-hop music.            |
| AgoraAudioReverbPresetVocalConcert    | The reverberation style typical of a concert hall.           |
| AgoraAudioReverbPresetKTV              | The reverberation style typical of a KTV venue.              |
| AgoraAudioReverbPresetStudio           | The reverberation style typical of a recording studio.       |
| AgoraAudioReverbPresetFxKTV           | The reverberation style typical of a KTV venue (enhanced).   |
| AgoraAudioReverbPresetFxVocalConcert | The reverberation style typical of a concert hall (enhanced). |
| AgoraAudioReverbPresetFxUncle         | The reverberation style typical of an uncle's voice.         |
| AgoraAudioReverbPresetFxSister        | The reverberation style typical of a sister's voice.         |
| AgoraAudioReverbPresetFxStudio        | The reverberation style typical of a recording studio (enhanced). |
| AgoraAudioReverbPresetFxPopular       | The reverberation style typical of popular music (enhanced). |
| AgoraAudioReverbPresetFxRNB           | The reverberation style typical of R&B music (enhanced).     |
| AgoraAudioReverbPresetFxPhonograph    | The reverberation style typical of a vintage phonograph.     |
| AgoraAudioReverbPresetVirtualStereo          | A reverberation style that adds a virtual stereo effect. The virtual stereo is an effect that renders the monophonic audio as the stereo audio, so that all users in the channel can hear the stereo voice effect. To achieve better virtual stereo reverberation, Agora recommends setting `profile` in `setAudioProfile` as `AgoraAudioProfileMusicHighQualityStereo(5)`. |

<div class="alert warning">When calling the <tt>setLocalVoiceReverbPreset</tt> method with an enumeration that begins with <tt>AgoraAudioReverbPresetFx</tt>, ensure that you set the <tt>profile</tt> parameter passed to the <tt>setAudioProfile</tt> as <tt>AgoraAudioProfileMusicHighQuality(4)</tt> or <tt>AgoraAudioProfileMusicHighQualityStereo(5)</tt>. Failure to do so prevents the <tt>setLocalVoiceReverbPreset</tt> method from setting the corresponding voice reverberation option.</div>

<div class="alert note">Do not use this method with <tt>setLocalVoiceChanger</tt>, because the method called later overrides the one called earlier.</div>

```swift
// swift
// Sets the reverberation style typical of a concert hall (enhanced).
agoraKit.setLocalVoiceChanger(.fxVocalConcert)
// Disables the voice reverberation.
agoraKit.setLocalVoiceReverbPreset(.off)
```

```objective-c
// objective-c
// Sets the reverberation style typical of a concert hall (enhanced).
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetFxVocalConcert];
// Disables the voice reverberation.
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetOff];
```

### Customize the voice effects

You can also customize the voice effects by adjusting the voice pitch, equalization, and reverberation settings.

The following sample code shows how to change from the original voice to Hulk's voice.

```swift
// swift
// Sets the pitch. The value ranges between 0.5 and 2.0. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
agoraKit.setLocalVoicePitch(1)

// Sets the local voice equalization.
// The first parameter sets the band frequency. The value ranges between 0 and 9. Each value represents the center frequency of the band: 31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, and 16k Hz
// The second parameter sets the gain of each band. The value range between -15 and 15 dB. The default value is 0.
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
// The first parameter sets the band frequency. The value ranges between 0 and 9. Each value represents the center frequency of the band: 31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, and 16k Hz
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

### API reference

- [`setLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`setLocalVoicePitch`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setLocalVoiceEqualizationOfBandFrequency`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceEqualizationOfBandFrequency:withGain:)
- [`setLocalVoiceReverbOfType`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbOfType:withValue:)

## Demo project

We provide open-source demo iOS projects on GitHub. You can try [Agora-RTC-With-Voice-Changer-iOS](https://github.com/AgoraIO/Advanced-Audio/tree/master/iOS%26macOS/Agora-RTC-With-Voice-Changer-iOS) for Swift and refer to the source code in [`EffectViewController.swift`](https://github.com/AgoraIO/Advanced-Audio/blob/master/iOS%26macOS/Agora-RTC-With-Voice-Changer-iOS/Agora-RTC-With-Voice-Changer-iOS/EffectViewController.swift).

## Considerations

- All methods mentioned in this guide work best with a human voice, and Agora recommends not using it for audio containing both music and a human voice.
- Agora recommends not using the methods for presetting voice effects and methods for customizing voice effects together. Failure to do so may result in undefined behaviors. If you want to use preset voice effects together with methods that customize voice effects, call the preset methods before the customization methods, or the method called later overrides the one called earlier.
- Agora recommends not using `setLocalVoiceChanger` and `setLocalVoiceReverbPreset` together, or the method called later overrides the one called earlier.
- To achieve a better voice effect quality, Agora recommends setting the `profile` parameter passed to the `setAudioProfile` as `AgoraAudioProfileMusicHighQuality(4)` or `AgoraAudioProfileMusicHighQualityStereo(5)` before calling `setLocalVoiceChanger` or `setLocalVoiceReverbPreset`.
