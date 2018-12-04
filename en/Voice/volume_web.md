
---
title: Adjust the Volume
description: How to adjust volume on Web
platform: Web
updatedAt: Tue Dec 04 2018 17:19:02 GMT+0000 (UTC)
---
# Adjust the Volume
## Feature Description
When using the Agora SDK, developers can adjust the recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume to 0.
## Implementation
Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Voice/web_prepare.md) for more information.
### Adjust the Local Volume

```
  // Sets the volume of the local stream to 50.
  localStream.setAudioVolume(50);
```

### Adjust the Playback Volume

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
