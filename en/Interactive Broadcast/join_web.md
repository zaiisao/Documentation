
---
title: Join a Channel
description: 
platform: Web
updatedAt: Tue Dec 11 2018 07:22:59 GMT+0000 (UTC)
---
# Join a Channel
Before joining the channel, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/web_prepare.md) for more information.

## Implementation

Once the client initialization is complete, call the  `client.join`  method in the `onSuccess` callback.

Pass the channel key, channel name, and user ID to the method parameters:

- `tokenOrKey`: For low-security requirements, pass null as the parameter value. For high-security requirements, pass the string of the token or Channel Key as the parameter value. See [Security Keys](../../en/Interactive%20Broadcast/token.md).
- `channel`: Channel name.
- `uid`: The user ID is an integer and should be unique. If you set the user ID to null, the Agora server assigns a user ID and returns it in the  `onSuccess` callback.

```javascript
client.join(<TOKEN_OR_KEY>, <CHANNEL_NAME>, <UID>, function(uid) {
  console.log("User " + uid + " join channel successfully");

}, function(err) {
  console.log("Join channel failed", err);
});
```

> Users with different App IDs cannot call each other even if they join the same channel.

## Next Steps

You are now in the channel and can start a voice call with the following step:

- [Publish and Subscrib to Streams](../../en/Voice/publish_web_audio.md)

For added requirements on the audio volume, audio effect or voice pitch, you can alse take the following steps:

- [Adjust the Volume](../../en/Voice/volume_web.md)
- [Play Audio Effects/Audio Mixing](../../en/Voice/effect_mixing_web.md)
- [Adjust the Pitch and Tone](../../en/Voice/audio_profile_web.md)
