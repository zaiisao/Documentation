
---
title: Play Audio Effects/Audio Mixing
description: How to use play effect and audio mixing methods
platform: Android
updatedAt: Fri Nov 23 2018 08:23:05 GMT+0000 (UTC)
---
# Play Audio Effects/Audio Mixing
## Feature Description
In a call or live broadcast, sometimes you need to play custom audio or music files to all the users in the channel, for example adding sound effects in a game, or playing a background music. Agora provides two groups of methods that supports playing audio effect files and audio mixing.
## Play Audio Effect Files

Audio effects are usually very short sounds. The play effect methods can be used to play sound snaps such as clapping and gunshots. You can play multiple audio effects at the same time, and preload the audio effect file for efficiency.

Agora SDK provides the `AudioEffectManager` class to manage the audio effects, including a series of methods. The audio effect is specified by the file path, but the `AudioEffectManager` uses the sound id to identify the audio effect file. The file is usually placed in **assets**. The SDK does not have a rule to define the sound id, and you need to ensure each audio effect file has a unique sound id. Common practices include automatically incrementing the id, and using the hashCode of the audio effect file.

### Implementation

```java
// Get the global audio effect manager
IAudioEffectManager manager = rtcEngine.getAudioEffectManager();
  
// Preload the audio effect (recommended). Note the file size and preload the file before joining the channel.
// Only mp3, aac, m4a, 3gp, and wav are supported
// You may need to record the matching between the sound ids and the file paths.
int id = 0;
manager.preloadEffect(id++, "path/to/effect1");
  
// You can preload multiple audio effects
manager.preloadEffect(id++, "path/to/effect2");
  
// Play an audio effect file
manager.playEffect(
0,  // The sound id of the audio effect file to play
"path/to/effect1",  // The file path to the audio effect
-1,   // The playback count. -1 means inifinite loop.
0.0,  // Set the spatial position of the effect. 0 means the effect shows ahead.
100,  // Set the volume. The value range is 0 to 100. 100 is the original volume.
true // Set whether to publish the audio effect.
);
  
// Pause all the audio effects
manager.pauseAllEffects();
  
// Get the volume of the audio effect. The value range is 0 to 100.
double volume = manager.getEffectsVolume();
  
// Ensure the audio effect's volume is always at least 80% of the original volume.
volume = volume < 80 ? 80 : volume;
manager.setEffectsVolume(volume);
  
// Resume playing the audio effect
manager.resumeAllEffects();
  
// Stop all the audio effect
manager.stopAllEffects();
  
// Release the preloaded audio effect from the memory.
manager.unloadAllEffects();
```

### API Reference

- [getAudioEffectManager](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#afd61b8d5e923f9e03cd419dcaf23b4af)

### Considerations

- Preloading is not mandatory, but to improve effeciency or to play the audio effect multiple times, Agora recommends you preload the audio effect.
- The above methods have return values. If the API fails, the return is < 0.

## Audio Mixing

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play longer background music, for example playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file playback stops.
Agora audio mixing supports the following options:
- Mix or replace the audio: Mixing means to mix the music file with the audio captured by the microphone and send to to other users; replacing means to replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.

### Implementation

```java
// Set audio mixing options
int loopCount = -1; // Loop the audio mixing file infinitely.If you use a positive integer, it represents the number of times to play the file.
boolean shouldLoop = true; // Sets whether other users can hear the audio mixing; if set to true, only the local user can hear the audio mixing.
boolean replaceMic = false; // The audio captured by the microphone is not replaced by the audio mixing file.
  
// Start audio mixing
rtcEngine.startAudioMixing("path/to/music", shouldLoop, replaceMic, loopCount);
  
// Adjust the volume of the audio mixing. The value range is 0 to 100. 100 represents the orginial volume (default).
int volume = 50;
rtcEngine.adjustAudioMixingVolume(volume);
  
// Get the duration of the current audio mixing file.
int duration = rtcEngine.getAudioMixingDuration();
// duration can be used to set the maximum length of the progress bar.
// seekBar.setMax(duration);
  
// Get the current progress of the audio mixing playback.
int currentPosition = rtcEngine.getAudioMixingCurrentPosition();
// You can set a timer to get and display the progress regularly.
// seekBar.setProgress(currentPosition);
  
// If the user drags the progress bar, you can get the progress in the callback of the seekBar and reset the current position of the music.
rtcEngine.setAudioMixingPosition(progress);
  
// Pause and resume the audio mixing.
rtcEngine.pauseAudioMixing();
rtcEngine.resumeAudioMixing();
  
// Stop the audio mixing and re-enable the microphone capturing.
rtcEngine.stopAudioMixing()；
```

### API References

- [startAudioMixing](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac56ceea1a143a4898382bce10b04df09)
- [stopAudioMixing](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#addb1cbc23b7f725eea6eedd18412854d)
- [adjustAudioMixingVolume](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a)
- [pauseAudioMixing](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab2d4fb72ec3031f59da72b55857e0da7)
- [resumeAudioMixing](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aedad78215c21f0a6acac7f155199f3ce)
- [getAudioMixingDuration](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8bbeb8a8b07e4e7b1a0a493f1c66998d)
- [getAudioMixingCurrentPosition](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a5119b0e6b356f867f7e13a6e1b2bb3e5)
- [setAudioMixingPosition](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a12c3dc250c86d54552c1589dfda2e002)

### Considerations

- The audio mixing methods require Android 4.2+, and API level 16+.
- Ensure you call these methods when you are in the channel.
- If you call these methods on an emulator, you can only play the mp3 files in the **/sdcard/** folder for audio mixing.
- The above methods have return values. If the API fails, the return is < 0.
