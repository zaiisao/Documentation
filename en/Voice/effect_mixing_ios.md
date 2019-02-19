
---
title: Play Audio Effects/Audio Mixing
description: How to play audio effects and enable audio mixing for iOS
platform: iOS,macOS
updatedAt: Fri Nov 23 2018 08:29:01 GMT+0000 (UTC)
---
# Play Audio Effects/Audio Mixing
## Feature Description
In a call or live broadcast, sometimes you need to play custom audio or music files to all the users in the channel, for example adding sound effects in a game, or playing a background music. Agora provides two groups of methods that supports playing audio effect files and audio mixing.

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/ios_video.md) for more information.
## Play Audio Effect Files

Audio effects are usually very short sounds. The play effect methods can be used to play sound snaps such as clapping and gunshots. You can play multiple audio effects at the same time, and preload the audio effect file for efficiency.

 The audio effect is specified by the file path, but the SDK uses the sound id to identify the audio effect file. The SDK does not have a rule to define the sound id, and you need to ensure each audio effect file has a unique sound id. Common practices include automatically incrementing the id, and using the hashCode of the audio effect file.

### Implementation

```swift
// swift
// Preload the audio effect (recommended). Note the file size and preload the file before joining the channel.
// Only mp3, aac, m4a, 3gp, and wav are supported
// You may need to record the matching between the sound ids and the file paths.
let soundId = 1
let filePath = "your filepath"

// You can preload multiple audio effects
agoraKit.preloadEffect(soundId, filePath: filePath)

// Play an audio effect file
let soundId = 1 // The sound id of the audio effect file to play
let filePath = "your filepath" // The file path to the audio effect
let loopCount = 1 // The playback count. -1 means inifinite loop.
let pitch = 1 // Set the pitch of the audio effect.
let pan = 1 // Set the spatial position of the effect. 0 means the effect shows ahead.
let gain = 0 // Set the volume. The value range is 0 to 100. 100 is the original volume.
let publish = true // Set whether to publish the audio effecet.
agoraKit.playEffect(Int32(soundId), filePath: filePath, loopCount: Int32(loopCount), pitch: pitch, pan: pan, gain: gain, publish: publish)

// Pause all the audio effects
agoraKit.pauseAllEffects()

// Get the volume of the audio effect. The value range is 0 to 100.
let volume = agoraKit.getEffectsVolume()

// Ensure the audio effect's volume is always at least 80% of the original volume.
volume = volume < 80 ? 80 : volume
agoraKit.setEffectsVolume(volume)

// Resume playing the audio effect
agoraKit.resumeAllEffects()

// Stop all the audio effects
agoraKit.stopAllEffects()
```

```objective-c
// objective-c
// Preload the audio effect (recommended). Note the file size and preload the file before joining the channel.
// Only mp3, aac, m4a, 3gp, and wav are supported
// You may need to record the matching between the sound ids and the file paths.
int soundId = 1
NSString *filePath = "your filepath"

// You can preload multiple audio effects
[agoraKit preloadEffect: soundId filePath: filePath];

// Play an audio effect file
int soundId = 1; // The sound id of the audio effect file to play
NSString *filePath = "your filepath"; // The file path to the audio effect
int loopCount = 1 // The playback count. -1 means inifinite loop.
double pitch = 1 // Set the pitch of the audio effect.
double pan = 1 // Set the spatial position of the effect. 0 means the effect shows ahead.
double gain = 0 // Set the volume. The value range is 0 to 100. 100 is the original volume.
BOOL publish = true // Set whether to publish the audio effecet.
[agoraKit playEffect: soundId filePath: filePath loopCount: loopCount, pitch: pitch, pan: pan, gain: gain, publish: publish];

// Pause all the audio effects
[agoraKit pauseAllEffects];

// Get the volume of the audio effect. The value range is 0 to 100.
int volume = [agoraKit getEffectsVolume];

// Ensure the audio effect's volume is always at least 80% of the original volume.
volume = volume < 80 ? 80 : volume
[agoraKit setEffectsVolume: volume];

// Resume playing the audio effect
[agoraKit resumeAllEffects];

// Stop all the audio effects
[agoraKit stopAllEffects];
```

### API Reference

- [playEffect](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/playEffect:filePath:loopCount:pitch:pan:gain:)
- [preloadEffect](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/preloadEffect:filePath:)
- [pauseAllEffects](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pauseAllEffects)
- [getEffectsVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getEffectsVolume)
- [setEffectsVolume](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setEffectsVolume:)
- [resumeAllEffects](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/resumeAllEffects)
- [stopAllEffects](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAllEffects)

### Considerations

- Preloading is not mandatory, but to improve effeciency or to play the audio effect multiple times, Agora recommends you preload the audio effect.
- The above methods have return values. If the API fails, the return is < 0.

## Audio Mixing

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play longer background music, for example playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file playback stops.
Agora audio mixing supports the following options:

- Mix or replace the audio: Mixing means to mix the music file with the audio captured by the microphone and send to to other users; replacing means to replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.

### Implementation

```swift
// swift
// loopback sets whether other users can hear the audio mixing; if set to true, only the local user can hear the audio mixing.
// replace sets whether the audio captured by the microphone is replaced by the audio mixing file. 
// cycle set as -1 means looping the audio mixing file infinitely.If you use a positive integer, it represents the number of times to play the file.
let filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3"
let loopback = false
let replace = false 
let cycle = 1 
  
// Start audio mixing
agoraKit.startAudioMixing(filePath, loopback: loopback, replace: replace, cycle: cycle)
```

```objective-c
// objective-c
// loopback sets whether other users can hear the audio mixing; if set to YES, only the local user can hear the audio mixing.
// replace sets whether the audio captured by the microphone is replaced by the audio mixing file. 
// cycle set as -1 means looping the audio mixing file infinitely.If you use a positive integer, it represents the number of times to play the file.
NSString *filePath = @"http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";
BOOL loopback = NO;
BOOL replace = NO;
NSInteger cycle = 1;

// Start audio mixing
[agoraKit startAudioMixing: filePath loopback: loopback replace: replace cycle: cycle];
```



### API References

- [startAudioMixing](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioMixing:loopback:replace:cycle:)

### Considerations

The above methods have return values. If the API fails, the return is < 0.
