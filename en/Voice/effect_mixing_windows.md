
---
title: Play Audio Effects/Audio Mixing
description: How to play audio effects and audio mixing
platform: Windows
updatedAt: Thu Nov 22 2018 06:28:03 GMT+0000 (UTC)
---
# Play Audio Effects/Audio Mixing
## Feature Description
In a call or live broadcast, sometimes you need to play custom audio or music files to all the users in the channel, for example adding sound effects in a game, or playing a background music. Agora provides two groups of methods that supports playing audio effect files and audio mixing.

## Play Audio Effect Files

Audio effects are usually very short sounds. The play effect methods can be used to play sound snaps such as clapping and gunshots. You can play multiple audio effects at the same time, and preload the audio effect file for efficiency.

The audio effect is specified by the file path, but the SDK uses the sound id to identify the audio effect file. The SDK does not have a rule to define the sound id, and you need to ensure each audio effect file has a unique sound id. Common practices include automatically incrementing the id, and using the hashCode of the audio effect file.

### Implementation

```c++
// Initialization
RtcEngineParameters rep(*lpAgoraEngine);

// Preload the audio effect (recommended). Note the file size and preload the file before joining the channel.
#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_ACP, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.preloadEffect(nSoundID, wdFilePath);
#else
  int nRet = rep.preloadEffect(nSoundID, filePath);
#endif

// Play the audio effect file. If you preload the audio effect, you need to specify the nSoundID.
#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_ACP, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.playEffect(nSoundID, // The unique sound ID of the audio effect
  wdFilePath, // File path to the audio effect
  nLoopCount, // The playback count. -1 means inifinite loop.
  dPitch, // Set the pitch of the audio effect
  dPan, // Set the spatial position of the effect. 0 means the effect shows ahead.
  nGain, // Set the volume. The value range is 0 to 100. 100is the original volume.
  TRUE // Set whether to publish the audio effecet.
#else
  int nRet = rep.playEffect(nSoundID, filePath, nLoopCount, dPitch, dPan, nGain, TRUE);
#endif

// Pause a sepcified audio effect
int nRet = rep.pauseEffect(nSoundID);

// Pause all the audio effects
int nRet = rep.pauseAllEffects();

// Resume playing the paused audio effect
int nRet = rep.resumeEffect(nSoundID);

// Resume playing all the audio effects
int nRet = rep.resumeAllEffects();

// Stop playing a sepcified audio effect
int nRet = rep.stopEffect(nSoundID);

// Stop playing all the audio effects
int nRet = rep.stopAllEffects();

// Release the preloaded audio effect from the memory
int nRet = rep.unloadEffect(nSoundID);
```

### API References

- [preloadEffect](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a61e4eac3b78f2774ef1b22d69bd4e166)
- [playEffect](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a26307c09cbbaecee3bd662294a935821)
- [pauseEffect](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a75fc09bdd0bd8b2bfe9c47770eb1e928)
- [pauseAllEffects](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a98ff58bdd2b8683bd27a1f75694641dc)
- [resumeEffect](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#adae083a10afd4b316a2071ba8d01ff80)
- [resumeAllEffects](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a66dd1578478dd3ca163768d1314cd50a)
- [stopEffect](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#ab0520529fe0ca4eb56d75ff4468e4a03)
- [stopAllEffects](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a7f742bd2262899a90f4a36205995419e)
- [unloadEffect](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#afd2cc4d59101cef1b5dc9296e604d047)

### Considerations

- Preloading is not mandatory, but to improve effeciency or to play the audio effect multiple times, Agora recommends you preload the audio effect.
- The above methods have return values. If the API fails, the return is < 0.

## Audio Mixing

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play longer background music, for example playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file playback stops.
Agora audio mixing supports the following options:

- Mix or replace the audio: Mixing means to mix the music file with the audio captured by the microphone and send to to other users; replacing means to replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.

### Implementation

```c++
LPCTSTR filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";

// Initialization
RtcEngineParameters rep(*lpAgoraEngine);

// Start audio mixing
#ifdef UNICODE
 CHAR wdFilePath[MAX_PATH];
 ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
int nRet = rep.startAudioMixing(wdFilePath, // Path to the audio mixing file.
 FALSE, // All users can hear the audio mixing.
  TRUE, // The audio captured by the microphone is replaced by the audio mixing file.
  1 // The number of playbacks of the file. If set to -1, the file loops infinitely.
  );
#else
int nRet = rep.startAudioMixing(filePath, FALSE, TRUE, 1);
#endif

// Stop audio mixing
int nRet = rep.stopAudioMixing();
```

### API References

- [startAudioMixing](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a13106dd42b618ab9d1a03f7ea1bc4f2f)
- [stopAudioMixng](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1e7955a19257fe8388f79213a1b7ad5b)

### Considerations

- Ensure you call these methods when you are in the channel.
- The above methods have return values. If the API fails, the return is < 0.
