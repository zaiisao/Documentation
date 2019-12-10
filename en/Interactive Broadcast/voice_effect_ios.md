
---
title: Set the Voice Changer and Reverberation Effects
description: How to set voice effects on iOS
platform: iOS
updatedAt: Tue Dec 10 2019 04:20:29 GMT+0800 (CST)
---
# Set the Voice Changer and Reverberation Effects
## Introduction 

In social and entertainment scenarios, users often need various voice effects to enhance interactive experiences. Agora provides multiple preset voice changers and reverberation effects. You can also flexibly change the users' voice, such as adjusting the pitch and setting the equalization and reverberation modes.

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md).

### Use a preset voice changer and reverberation effect

You can use one of the following preset voice changer options by calling `setLocalVoiceChanger`:

- An old man's voice.
- A little boy's voice.
- A little girl's voice.
- Zhu Bajie's voice (Zhu Bajie is a character from Journey to the West who has a voice like a growling bear).
- Ethereal vocal effects.
- Hulk's voice.

```swift
// swift
// Set the voice changer as old man
agoraKit.setLocalVoiceChanger(.oldMan)

// Turn off the voice changer
agoraKit.setLocalVoiceChanger(.off)
```

```objective-c
// objective-c
// Set the voice changer as old man
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOldMan];

// Turn off the voice changer
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

You can use one of the following preset reverberation effects by calling `setLocalVoiceReverbPreset`:

- Pop music
- R&B
- Rock music
- Hip-hop
- Pop concert
- KTV
- Recording studio

```swift
// swift
// Set the reverberation preset as pop
agoraKit.setLocalVoiceReverbPreset(.popular)

// Turn off the reverberation
agoraKit.setLocalVoiceReverbPreset(.off)
```

```objective-c
// objective-c
// Set the reverberation preset as pop
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetPopular];

// Turn off the reverberation
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetOff];
```

### Customize the voice effects

You can also customize the voice effects by adjusting the voice pitch, equalization, and reverberation settings.

The following sample code shows how to set the FM voice effect.

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

### API Reference

- [`setLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`setLocalVoicePitch`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setLocalVoiceEqualizationOfBandFrequency`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceEqualizationOfBandFrequency:withGain:)
- [`setLocalVoiceReverbOfType`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbOfType:withValue:)

## Sample Code

Agora provides an open source sample code that implements voice changer and reverberation. You can go to the [Chatroom GitHub Repo](https://github.com/AgoraIO-Usecase/Chatroom/tree/master/iOS) to download it.

## Considerations
The API methods have return values. If the method call fails, the return value is < 0.

