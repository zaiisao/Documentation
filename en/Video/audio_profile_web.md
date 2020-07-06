
---
title: Set the Audio Profile
description: How to set high-quality audio on Web
platform: Web
updatedAt: Mon Jul 06 2020 09:24:04 GMT+0800 (CST)
---
# Set the Audio Profile
## Introduction 

High-fidelity audio is essential for professional audio scenarios, such as for podcasts and singing competitions. For example, podcasts require stereo and high-fidelity audio. High-fidelity audio refers to an audio profile with a sample rate of 48 kHz and a bitrate of 192 Kbps.


## Implementation

Before adjusting the audio volume, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Video/start_call_web.md) or [Start Live Interactive Streaming](../../en/Video/start_live_web.md).

The Agora Web SDK provides the `setAudioProfile` method for developers to set appropriate audio profiles according to the scenarios. The `profile` parameter sets the sampling rate, bitrate, and encoding mode.

### API call sequence

The following diagram shows how to set the audio profile:

![](https://web-cdn.agora.io/docs-files/1569380760195)

### Sample code

```javascript
  // Sets the audio profile with a 48-kHz sampling rate, stereo sound, and 192-Kbps bitrate.
  localStream.setAudioProfile("high_quality_stereo");
  localStream.init(function(){
   // Initialization successful.
  });
```

### API reference

- [setAudioProfile](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile).

## Considerations

- Call the `setAudioProfile` method before `Stream.init`.
