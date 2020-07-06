
---
title: Adjust the Volume
description: How to adjust volume on Web
platform: Web
updatedAt: Mon Jul 06 2020 09:21:16 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction
The Agora RTC SDK enables you to manage the volume of the recorded audio or of the audio playback according to your actual scenario. For example, to mute a remote user in a one-to-one call, you can set the audio playback volume as 0.
## Implementation
Before adjusting the audio volume, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Video/start_call_web.md) or [Start Live Interactive Streaming](../../en/Video/start_live_web.md).

### Sample code

#### Adjust the playout volume

```
 client.on("stream-subscribed", function(evt){
  var stream = evt.stream;
  // Sets the volume of the remote stream to 50.
  stream.setAudioVolume(50);
 });
```

#### Mute the remote user

```
 client.on("stream-subscribed", function(evt){
  var stream = evt.stream;
  // Mutes the remote stream.
  stream.setAudioVolume(0);
 });
```

### API reference

- [`setAudioVolume`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.stream.html#setaudiovolume)

## Considerations

- If the volume is set too high, noise may occur on some devices.

## Reference

When adjusting the audio volume, you can also refer to the following articles:

- [How to solve the problem of low volume?](https://docs.agora.io/en/faq/audio_low)
