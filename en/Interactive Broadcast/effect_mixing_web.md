
---
title: Play Audio Effects/Audio Mixing
description: How to enable audio mixing for Web
platform: Web
updatedAt: Tue Mar 19 2019 09:24:32 GMT+0000 (UTC)
---
# Play Audio Effects/Audio Mixing
## Introduction

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play background music. For example, playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file stops playing.
Agora audio mixing supports the following options:

- Mix or replace the audio: 
	- Mix the music file with the audio captured by the microphone and send it to other users.
	- Replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.

## Implementation
Before proceeding, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md).

```javascript
// Sets the audio mixing options.
// filePath is a mandatory parameter used to indicate an online URL of the mixing audio.
// cycle is an optional parameter used to indicate the number of playback loops and it needs to be a positive integer. The web browser needs to be Google Chrome 65+.
// replace is an optional parameter used to indicate if the audio mixing replaces the original audio. 
// playTime is a mandatory parameter used to set the starting position of mixing audio playback. 0 means playing the mixing file from the beginning.
var options = {
      filePath: "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3", 
      cycle: 1, 
      replace: false, 
      playTime:0 
}

// Starts audio mixing.
localStream.startAudioMixing(options, function(err){
     if (err){
             console.log("Failed to start audio mixing. " + err);
      }
});

// Adjusts the volume of audio mixing. The value of volume ranges between 1 and 100.
localStream.adjustAudioMixingVolume(volume);

// Gets the playback position (ms) of the mixed audio.
localStream.getAudioMixingCurrentPosition();

// Sets the playback position of the mixed audio.
localStream.setAudioMixingPosition(pos);

// Pauses audio mixing.
localStream.pauseAudioMixing();

// Resumes audio mixing.
localStream.resumeAudioMixing();

// Gets the duration (ms) of audio mixing. 
localStream.getAudioMixingDuration();

// Stops audio mixing.
localStream.stopAudioMixing();
```

### API Reference

- [`startAudioMixing`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing)
- [`stopAudioMixing`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#stopaudiomixing)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#adjustaudiomixingvolume)
- [`pauseAudioMixing`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#pauseaudiomixing)
- [`resumeAudioMixing`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#resumeaudiomixing)
- [`getAudioMixingDuration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiomixingduration)
- [`getAudioMixingCurrentPosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiomixingcurrentposition)
- [`setAudioMixingPosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudiomixingposition)

## Considerations

- Agora Web SDK supports using only online music files for audio mixing.
- The audio mixing methods support the following browsers:
  - Safari 12+
  - Google Chrome 65+
  - Firefox
- Call the methods when you are in the channel.
- Some parameters are supported by specific web browsers. See [startAudioMixing](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing) for details.
