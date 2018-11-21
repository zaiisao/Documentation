
---
title: Play Audio Effects/Audio Mixing
description: How to enable audio mixing for Web
platform: Web
updatedAt: Wed Nov 21 2018 09:22:18 GMT+0000 (UTC)
---
# Play Audio Effects/Audio Mixing
## Feature Description

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play longer background music, for example playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file playback stops.
Agora audio mixing supports the following options:

- Mix or replace the audio: Mixing means to mix the music file with the audio captured by the microphone and send to to other users; replacing means to replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.
## Implementation

```javascript
// Set the audio mixing options
var options = {  
   filePath: "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3", // Specify the file path of the audio mixing file.  
   cycle: 1, // Set the number of times to play the audio mixing file. Only supports Chrome 65+.  
   playTime: 0, // Set the playback position (ms) of the audio mixing file. 0 means to play the file from the beginning.   
   replace: false // Set whether to replace the audio captured by the microphone with the audio mixing file. 
  }
  // Start audio mixing
  localStream.startAudioMixing(options, function(err){
   if (err){
     throw err;
   }
   // Stop audio mixing after 5 seconds
   setTimeout(function(){
    localStream.stopAudioMixing();
   }, 5 * 1000);
  });      
```
## Considerations

- Agora Web SDK supports only online music files for audio mixing
- The audio mixing methods support the following browsers:
  - Safari 12+
  - Chrome 65+
  - Latest Firefox
- Call this method when you are in the channel. Otherwise, issues may occur.
- Some parameters are supported by specific browsers, see [startAudioMixing](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing) for details.
