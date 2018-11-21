
---
title: Play Audio Effects/Audio Mixing
description: How to play audio effects and audio mixing
platform: Windows
updatedAt: Wed Nov 21 2018 08:20:38 GMT+0000 (UTC)
---
# Play Audio Effects/Audio Mixing
## Feature Description
In a call or live broadcast, sometimes you need to play custom audio or music files to all the users in the channel, for example adding sound effects in a game, or playing a background music. Agora provides two groups of methods that supports playing audio effect files and audio mixing.

## Play Audio Effect Files

Audio effects are usually very short sounds. The play effect methods can be used to play sound snaps such as clapping and gunshots. You can play multiple audio effects at the same time, and preload the audio effect file for efficiency.

The audio effect is specified by the file path, but the SDK uses the sound id to identify the audio effect file. The SDK does not have a rule to define the sound id, and you need to ensure each audio effect file has a unique sound id. Common practices include automatically incrementing the id, and using the hashCode of the audio effect file.

### Implementation

```c++
RtcEngineParameters rep(*lpAgoraEngine);

// 1. Preload the audio effect file, optional

#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_ACP, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.preloadEffect(nSoundID, wdFilePath);
#else
  int nRet = rep.preloadEffect(nSoundID, filePath);
#endif

// 2. Play the audio effect file. If you preload the audio effect, you need to specify the nSoundID.

#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_ACP, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.playEffect(nSoundID, wdFilePath, nLoopCount, dPitch, dPan, nGain, TRUE /* publish */);
#else
  int nRet = rep.playEffect(nSoundID, filePath, nLoopCount, dPitch, dPan, nGain, TRUE /* publish */);
#endif

// 3. Pause a specific audio effect

nRet = rep.pauseEffect(nSoundID);

// 4. Pause all the audio effects

nRet = rep.pauseAllEffects();

// 5. Resume playing a specific audio effect

nRet = rep.resumeEffect(nSoundID);

// 6. Resume playing all the audio effects

nRet = rep.resumeAllEffects();

// 7. Stop playing a specific audio effect

nRet = rep.unloadEffect(nSoundID);

// 8. Release the preloaded audio effect from the memory.

nRet = rep.unloadEffect(nSoundID);
```

### Considerations
- Ensure the file path is correct and the audio effect file is complete.
- If the `publish` parameter is set as `TRUE`,  other users in the channel will hear the audio effect.
- The above methods have return values. If the API fails, the return is < 0.

## Audio Mixing

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play longer background music, for example playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file playback stops.
Agora audio mixing supports the following options:

- Mix or replace the audio: Mixing means to mix the music file with the audio captured by the microphone and send to to other users; replacing means to replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.

### Implementation

```c++
  LPCTSTR filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";

  int nRet = 0;
  RtcEngineParameters rep(*lpAgoraEngine);

  // 1. Start audio mixing

  #ifdef UNICODE
   CHAR wdFilePath[MAX_PATH];
   ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
   nRet = rep.startAudioMixing(wdFilePath, FALSE /* loopback */, TRUE /* replace */, 1 /* repeat times */);
  #else
   nRet = rep.startAudioMixing(filePath, FALSE /* loopback */, TRUE /* replace */, 1 /* repeat times */);
  #endif

  // 2. Stop audio mixing

  nRet = rep.stopAudioMixing();   
```

### Considerations

- If `loopback` is set as `TRUE` , only the local user can hear the audio mixing.
- If `replace` is set as `TRUE`, the audio mixing file will replace the audio captured by the microphone.
- `repeat times` represents the number of times to play the audio mixing file. `-1` means infinite loop. 
- Ensure you call these methods when you are in the channel.
- The above methods have return values. If the API fails, the return is < 0.
