
---
title: Adjust the Volume
description: How to adjust volume on Web
platform: Web
updatedAt: Fri Sep 20 2019 04:26:14 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction
When using the Agora SDK, you can adjust the audio recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume as 0.
## Implementation
Before proceeding, ensure that you prepare the development environment. See [Integrate the SDK](../../en/Video/web_prepare.md).

### Adjust the Playout Volume

```
 client.on("stream-subscribed", function(evt){
  var stream = evt.stream;
  // Sets the volume of the remote stream to 50.
  stream.setAudioVolume(50);
 });
```

### Mute the Remote User

```
 client.on("stream-subscribed", function(evt){
  var stream = evt.stream;
  // Mutes the remote stream.
  stream.setAudioVolume(0);
 });
```

## Considerations

- If the volume is set too high, noise may occur on some devices.
