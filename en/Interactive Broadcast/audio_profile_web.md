
---
title: Set the Stereo/High-fidelity Audio Profile
description: How to set high-quality audio on Web
platform: Web
updatedAt: Tue Dec 04 2018 22:15:28 GMT+0000 (UTC)
---
# Set the Stereo/High-fidelity Audio Profile
## Feature Description 

High-fidelity audio is essential for professional audio scenarios, such as for podcasts and singing competitions. For example, podcasts require stereo and high-fidelity audio. High-fidelity audio refers to an audio profile with a 48-KHz sampling rate and a 192-Kbps bitrate. 


## Implementation
Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md) for more information.

Agora Web SDK provides the [setAudioProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile) method for developers to set appropriate audio profiles according to the scenarios. The `profile` parameter sets the sampling rate, bitrate, and encode mode.

```javascript
  // Set the audio profile of 48 KHz sampling rate, stereo, and 192 Kbps bitrate.
  localStream.setAudioProfile("high_quality_stereo");
  localStream.init(function(){
   // init successful
  });
```

> For more options, see  [setAudioProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile).

## Considerations

- Call this method before `Stream.init`.
