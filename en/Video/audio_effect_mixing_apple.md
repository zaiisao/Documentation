
---
title: Play Audio Effects/Audio Mixing File
description: How to play audio effects and enable audio mixing for iOS
platform: iOS,macOS
updatedAt: Fri Oct 16 2020 10:10:55 GMT+0800 (CST)
---
# Play Audio Effects/Audio Mixing File
## Introduction
In a call or interactive live streaming, you may need to play custom audio or music files to all the users in the channel. For example, adding sound effects in a game, or playing background music. We provide two groups of methods for playing audio effect files and audio mixing.

Before proceeding, ensure that you implement a basic call or live interactive streaming in your project. See the Quickstart Guides for details:

- iOS: [Start a Call](../../en/Video/start_call_ios.md)/[Start Live Interactive Streaming](../../en/Video/start_live_ios.md)
- macOS: [Start a call](../../en/Video/start_call_mac.md)/[Start Live Interactive Streaming](../../en/Video/start_live_mac.md)

## Sample project

Agora provides an open-source demo project that has implemented audio mixing on GitHub. You can download the project and view the source code:

- iOS: [AudioMixing](https://github.com/AgoraIO/API-Examples/blob/master/iOS/APIExample/Examples/Advanced/AudioMixing/AudioMixing.swift)
- macOS: [AudioMixing](https://github.com/AgoraIO/API-Examples/blob/master/macOS/APIExample/Examples/Advanced/AudioMixing/AudioMixing.swift)

## Play audio effect files

The play audio effect methods can be used to play ambient sound, such as clapping and gunshots. You can play multiple audio effects at the same time, and preload the audio effect file for efficiency.

The audio effect file is specified by the file path, but the SDK uses the sound ID to identify the audio effect file. The SDK does not have any rule to define the sound ID. You need to ensure that each audio effect file has a unique sound ID. You may increment the ID and use the hashCode of the audio effect file.

### Implementation

```swift
// Swift
// Preloads the audio effect (recommended). Note the file size and preload the file before joining the channel.
// Only mp3, aac, m4a, 3gp, and wav files are supported.
// You may need to record the correlation between the sound IDs and the file paths.
let soundId = 1
let filePath = "your filepath"

// You can preload multiple audio effects.
agoraKit.preloadEffect(soundId, filePath: filePath)

// Plays an audio effect file.
let soundId = 1 // The sound ID of the audio effect file to be played.
let filePath = "your filepath" // The file path of the audio effect file.
let loopCount = 1 // The number of playback loops. -1 means an infinite loop.
let pitch = 1 // Sets the pitch of the audio effect.
let pan = 1 // Sets the spatial position of the audio effect. 0 means the effect shows ahead.
let gain = 100 // Sets the volume. The value ranges between 0 and 100. 100 is the original volume.
let publish = true // Sets whether to publish the audio effect.
agoraKit.playEffect(Int32(soundId), filePath: filePath, loopCount: Int32(loopCount), pitch: pitch, pan: pan, gain: gain, publish: publish)

// Pauses all audio effects.
agoraKit.pauseAllEffects()

// Gets the volume of the audio effect. The value ranges between 0 and 100.
let volume = agoraKit.getEffectsVolume()

// Ensures that the volume of all audio effect files is at least 80% of the original volume.
volume = volume < 80 ? 80 : volume
agoraKit.setEffectsVolume(volume)

// Sets the volume of a specified audio effect file as 50% of the original volume.
agoraKit.setVolumeOfEffect(soundId:"1", 50)

// Resumes playing the audio effect.
agoraKit.resumeAllEffects()

// Stops playing all audio effects.
agoraKit.stopAllEffects()
```

```objective-c
// Objective-C
// Preloads the audio effect (recommended). Note the file size and preload the file before joining the channel.
// Only mp3, aac, m4a, 3gp, and wav files are supported.
// You may need to record the correlation between the sound IDs and the file paths.
int soundId = 1;
NSString *filePath = "your filepath";

// You can preload multiple audio effects.
[agoraKit preloadEffect: soundId filePath: filePath];

// Plays an audio effect file.
int soundId = 1; // The sound ID of the audio effect file to be played.
NSString *filePath = "your filepath"; // The file path of the audio effect file.
int loopCount = 1; // The number of playback loops. -1 means an infinite loop.
double pitch = 1; // Sets the pitch of the audio effect.
double pan = 1; // Sets the spatial position of the audio effect. 0 means the effect shows ahead.
double gain = 100; // Sets the volume. The value ranges between 0 and 100. 100 is the original volume.
BOOL publish = true; // Sets whether to publish the audio effect.
[agoraKit playEffect: soundId filePath: filePath loopCount: loopCount pitch: pitch pan: pan gain: gain publish: publish];

// Pauses all audio effects.
[agoraKit pauseAllEffects];

// Gets the volume of the audio effect. The value ranges between 0 and 100.
int volume = [agoraKit getEffectsVolume];

// Ensures that the volume of all audio effect files is at least 80% of the original volume.
volume = volume < 80 ? 80 : volume;
[agoraKit setEffectsVolume: volume];

// Sets the volume of a specified audio effect file as 50% of the original volume.
[agoraKit setVolumeOfEffect: soundId:@"1" volume:50];

// Resumes playing the audio effect.
[agoraKit resumeAllEffects];

// Stops playing all audio effects.
[agoraKit stopAllEffects];
```

### API reference

- [playEffect](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/playEffect:filePath:loopCount:pitch:pan:gain:)
- [preloadEffect](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/preloadEffect:filePath:)
- [pauseAllEffects](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pauseAllEffects)
- [getEffectsVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getEffectsVolume)
- [setEffectsVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setEffectsVolume:)
- [`setVolumeOfEffect`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVolumeOfEffect:withVolume:)
- [resumeAllEffects](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/resumeAllEffects)
- [stopAllEffects](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAllEffects)

### Considerations

- Preloading the audio effect is not mandatory. Agora recommends you preload the audio effect to improve the efficiency or to play the audio effect multiple times.
- The API methods have return values. If the method call fails, the return value is < 0.

## Audio mixing

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play background music. For example, playing music in a live interactive streaming. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file stops playing.
Agora audio mixing supports the following options:

- Mix or replace the audio: 
	- Mix the music file with the audio captured by the microphone and send it to other users.
	- Replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.
- Adjust the audio volume: Adjust the playback volume of local and/or remote music files at the same time or separately.
- Adjust the audio pitch: Adjust the pitch of the local music file or the pitch of the local voice separately.


### Implementation

```swift
// Swift
// loopback sets whether other users can hear the audio mixing. If loopback is set as true, only the local user can hear the audio mixing.
// replace sets whether the audio captured by the microphone is replaced by the audio mixing file. 
// Setting cycle as -1 means looping the audio mixing file infinitely. Setting cycle as a positive integer means the number of times to play the file.
let filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3"
let loopback = false
let replace = false 
let cycle = 1 
  
// Starts audio mixing.
agoraKit.startAudioMixing(filePath, loopback: loopback, replace: replace, cycle: cycle)

// Sets the audio mixing volume for both local and remote playback as 50% of the original volume.
agoraKit.adjustAudioMixingVolume(50)
```

```bjective-c
// Objective-C
// loopback sets whether other users can hear the audio mixing. If loopback is set as YES, only the local user can hear the audio mixing.
// replace sets whether the audio captured by the microphone is replaced by the audio mixing file. 
// Setting cycle as -1 means looping the audio mixing file infinitely. Setting cycle as a positive integer means the number of times to play the file.
NSString *filePath = @"http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";
BOOL loopback = NO;
BOOL replace = NO;
NSInteger cycle = 1;

// Starts audio mixing.
[agoraKit startAudioMixing: filePath loopback: loopback replace: replace cycle: cycle];


// Sets the audio mixing volume for both local and remote playback as 50% of the original volume.
[agoraKit adjustAudioMixingVolume: 50];
```

You can also use `startAudioMixingPlayoutVolume` and `adjustAudioMixingPublishVolume` to adjust the audio mixing volume for the local and remote playback respectively. The value range of `volume` id [0, 100].

```swift
// Swift
// Sets the audio mixing volume for remote playback as 50% of the original volume.
agoraKit.adjustAudioMixingPublishVolume(50)
// Sets the audio mixing volume for local playback as 50% of the original volume.
agoraKit.adjustAudioMixingPlayoutVolume(50)
```

```objective-c
// Objective-C
// Sets the audio mixing volume for remote playback as 50% of the original volume.
[agoraKit adjustAudioMixingPublishVolume: 50];
// Sets the audio mixing volume for local playback as 50% of the original volume.
[agoraKit adjustAudioMixingPlayoutVolume: 50];
```


### API reference

- [`startAudioMixing`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioMixing:loopback:replace:cycle:)
- [`stopAudioMixing`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioMixing)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`setLocalVoicePitch`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setAudioMixingPitch`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioMixingPitch:)
- [`pauseAudioMixing`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pauseAudioMixing)
- [`resumeAudioMixing`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/resumeAudioMixing)
- [`getAudioMixingDuration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingDuration)
- [`getAudioMixingCurrentPosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingCurrentPosition)
- [`setAudioMixingPosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioMixingPosition:)

### Considerations

The API methods have return values. If the method call fails, the return value is < 0.
