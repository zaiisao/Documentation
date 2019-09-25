
---
title: Adjust the Volume
description: How to adjust volume on Web
platform: Web
updatedAt: Wed Sep 25 2019 10:07:17 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction
When using the Agora SDK, you can adjust the audio recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume as 0.
## Implementation
Before adjusting the audio volume, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_web.md) or [Start a Live Broadcast](../../en/Audio%20Broadcast/start_live_web.md).

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

- [`setAudioVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudiovolume)

## Considerations

- If the volume is set too high, noise may occur on some devices.
