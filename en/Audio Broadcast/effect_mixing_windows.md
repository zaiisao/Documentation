
---
title: Play Audio Effects/Audio Mixing
description: How to play audio effects and audio mixing
platform: Windows
updatedAt: Wed Apr 03 2019 06:27:32 GMT+0800 (CST)
---
# Play Audio Effects/Audio Mixing
## Introduction
In a call or live broadcast, you may need to play custom audio or music files to all the users in the channel. For example, adding sound effects in a game, or playing background music. We provide two groups of methods for playing audio effect files and audio mixing.

Before proceeding, ensure that you prepare the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/windows_video.md).

## Play Audio Effect Files

The play audio effect methods can be used to play audio effects, such as clapping and gunshots. You can play multiple audio effects at the same time, and preload the audio effect file for efficiency.

The audio effect file is specified by the file path, but the SDK uses the sound ID to identify the audio effect file. The SDK does not have the rule to define the sound ID. You need to ensure that each audio effect file has a unique sound ID. Common practices include automatically incrementing the ID, and using the hashCode of the audio effect file.

### Implementation

```c++
// Initialization.
RtcEngineParameters rep(*lpAgoraEngine);

// Preloads the audio effect (recommended). Note the file size and preload the file before joining the channel.
#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.preloadEffect(nSoundID, wdFilePath);
#else
  int nRet = rep.preloadEffect(nSoundID, filePath);
#endif

// Plays the audio effect file. If you preload the audio effect, you need to specify nSoundID.
#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.playEffect(nSoundID, // The unique sound ID of the audio effect.
  wdFilePath, // File path of the audio effect file.
  nLoopCount, // The number of playback loops. -1 means an infinite loop.
  dPitch, // Sets the pitch of the audio effect.
  dPan, // Sets the spatial position of the audio effect. 0 means the effect shows ahead.
  nGain, // Sets the volume. The value ranges between 0 and 100. 100 is the original volume.
  TRUE // Sets whether to publish the audio effect.
#else
  int nRet = rep.playEffect(nSoundID, filePath, nLoopCount, dPitch, dPan, nGain, TRUE);
#endif

// Pauses a specified audio effect.
int nRet = rep.pauseEffect(nSoundID);

// Pause all audio effects.
int nRet = rep.pauseAllEffects();

// Resumes playing the paused audio effect.
int nRet = rep.resumeEffect(nSoundID);

// Resumes playing all audio effects.
int nRet = rep.resumeAllEffects();

// Stops playing a specified audio effect.
int nRet = rep.stopEffect(nSoundID);

// Stops playing all audio effects.
int nRet = rep.stopAllEffects();

// Releases the preloaded audio effect from the memory.
int nRet = rep.unloadEffect(nSoundID);
```

### API Reference

- [`preloadEffect`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a61e4eac3b78f2774ef1b22d69bd4e166)
- [`playEffect`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a26307c09cbbaecee3bd662294a935821)
- [`pauseEffect`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a75fc09bdd0bd8b2bfe9c47770eb1e928)
- [`pauseAllEffects`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a98ff58bdd2b8683bd27a1f75694641dc)
- [`resumeEffect`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#adae083a10afd4b316a2071ba8d01ff80)
- [`resumeAllEffects`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a66dd1578478dd3ca163768d1314cd50a)
- [`stopEffect`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#ab0520529fe0ca4eb56d75ff4468e4a03)
- [`stopAllEffects`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a7f742bd2262899a90f4a36205995419e)
- [`unloadEffect`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#afd2cc4d59101cef1b5dc9296e604d047)

### Considerations

-  Preloading the audio effect is not mandatory. Agora recommends you preload the audio effect to improve the efficiency or to play the audio effect multiple times.
- The API methods have return values. If the method call fails, the return value is < 0.

## Audio Mixing

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play background music. For example, playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file stops playing.
Agora audio mixing supports the following options:

- Mix or replace the audio: 
	- Mix the music file with the audio captured by the microphone and send it to other users.
	- Replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.
- State change notification: reports when the state of the audio mixing file changes, for example when the audio mixing file pauses playing or resumes playing.


### Implementation

```c++
LPCTSTR filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";

// Initialization.
RtcEngineParameters rep(*lpAgoraEngine);

// Starts audio mixing.
#ifdef UNICODE
 CHAR wdFilePath[MAX_PATH];
 ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
int nRet = rep.startAudioMixing(wdFilePath, // Path to the audio mixing file.
 FALSE, // All users can hear the audio mixing.
  TRUE, // The audio captured by the microphone is replaced by the audio mixing file.
  1 // The number of playback loops of the file. If set to -1, the file loops infinitely.
  );
#else
int nRet = rep.startAudioMixing(filePath, FALSE, TRUE, 1);
#endif

// Stops audio mixing.
int nRet = rep.stopAudioMixing();
```

### API Reference

- [`startAudioMixing`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a13106dd42b618ab9d1a03f7ea1bc4f2f)
- [`stopAudioMixng`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1e7955a19257fe8388f79213a1b7ad5b)
- [`onAudioMixingStateChanged`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a298389513bfaa50af4277fc3296e3f22)

### Considerations

- Ensure that you call the methods when you are in the channel.
- The API methods have return values. If the method call fails, the return value is < 0.
