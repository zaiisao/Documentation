
---
title: Set the Stereo/High-fidelity Audio Profile
description: How to set high-quality audio on Web
platform: Web
updatedAt: Fri Aug 16 2019 04:33:52 GMT+0800 (CST)
---
# Set the Stereo/High-fidelity Audio Profile
## Introduction 

High-fidelity audio is essential for professional audio scenarios, such as for podcasts and singing competitions. For example, podcasts require stereo and high-fidelity audio. High-fidelity audio refers to an audio profile with a 48-KHz sampling rate and a 192-Kbps bitrate. 


## Implementation
Before proceeding, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/web_prepare.md).

The Agora Web SDK provides the [setAudioProfile](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile) method for developers to set appropriate audio profiles according to the scenarios. The `profile` parameter sets the sampling rate, bitrate, and encoding mode.

```javascript
  // Sets the audio profile with a 48-KHz sampling rate, stereo sound, and 192-Kbps bitrate.
  localStream.setAudioProfile("high_quality_stereo");
  localStream.init(function(){
   // Initialization successful.
  });
```

> For more options, see [setAudioProfile](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile).

## Considerations

- Call the [setAudioProfile](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile) method before `Stream.init`.
