
---
title: Play Audio Effects/Audio Mixing
description: How to enable audio mixing for Web
platform: Web
updatedAt: Fri Nov 23 2018 08:30:46 GMT+0000 (UTC)
---
# Play Audio Effects/Audio Mixing
## Feature Description

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play longer background music, for example playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file playback stops.
Agora audio mixing supports the following options:

- Mix or replace the audio: Mixing means to mix the music file with the audio captured by the microphone and send to to other users; replacing means to replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.

## Implementation
Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md) for more information.

```javascript
// Set audio mixing options
// filePath is a mandatory parameter to indicate an online url of mixing audio.
// cycle is an optional parameter to indicate the number of playback and it needs to be a positive integer. The browser needs to be Chrome 65+.
// replace is an optional parameter to indicate if the audio mixing replaces the original audio. 
// playTime is a mandotary parameter to set the starting position of mixing audio. 0 means playing the mixing file from the beginning.
var options = {
      filePath: "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3", 
      cycle: 1, 
      replace: false, 
      playTime:0 
}

// Start audio mixing
localStream.startAudioMixing(options, function(err){
     if (err){
             console.log("Failed to start audio mixing. " + err);
      }
});

// Adjust the volume of audio mixing. The volume is a number with the value range [1, 100].
localStream.adjustAudioMixingVolume(volume);

// Retrieve the position of the mixed audio. The unit of return value is ms.
localStream.getAudioMixingCurrentPosition();

// Set the mixing audio position
localStream.setAudioMixingPosition(pos);

// Pause the audio mixing
localStream.pauseAudioMixing();

// Resume the audio mixing
localStream.resumeAudioMixing();

// Get the current position of audio mixing. The unit of returned value is ms.
localStream.getAudioMixingCurrentPosition();

// Get the duration of audio mixing. The unit of returned value is ms.
localStream.getAudioMixingDuration();

// Stop audio mixing
localStream.stopAudioMixing();
```

### API References

- [startAudioMixing](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing)
- [stopAudioMixing](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#stopaudiomixing)
- [adjustAudioMixingVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#adjustaudiomixingvolume)
- [pauseAudioMixing](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#pauseaudiomixing)
- [resumeAudioMixing](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#resumeaudiomixing)
- [getAudioMixingDuration](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiomixingduration)
- [getAudioMixingCurrentPosition](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiomixingcurrentposition)
- [setAudioMixingPosition](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudiomixingposition)

## Considerations

- Agora Web SDK supports only online music files for audio mixing
- The audio mixing methods support the following browsers:
  - Safari 12+
  - Chrome 65+
  - Latest Firefox
- Call this method when you are in the channel. Otherwise, issues may occur.
- Some parameters are supported by specific browsers, see [startAudioMixing](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing) for details.
