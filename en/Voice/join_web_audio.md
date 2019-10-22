
---
title: Join a Channel
description: 
platform: Web
updatedAt: Fri Jul 19 2019 09:22:16 GMT+0800 (CST)
---
# Join a Channel
Before joining a channel, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Voice/web_prepare.md).

## Implementation

Once the client initialization is complete, call the `client.join`  method in the `onSuccess` callback.

Pass the channel key, channel name, and user ID to the method parameters:

- `tokenOrKey`: Pass a token that identifies the role and privilege of the user. 
	- For the testing environment, we recommend usign a Temp Token generated on Console. See [Get a Temp Token](../../en/Voice/token.md).
	- For the production environment, we recommend using a Token generated at your server. For how to generate a token, see [Token Security](../../en/Voice/token_server.md). 
- `channel`: Channel name.
- `uid`: The user ID is an integer and should be unique. If you set the user ID to null, the Agora server assigns a user ID and returns it in the `onSuccess` callback.

```javascript
client.join(<TOKEN_OR_KEY>, <CHANNEL_NAME>, <UID>, function(uid) {
  console.log("User " + uid + " join channel successfully");

}, function(err) {
  console.log("Join channel failed", err);
});
```

> Users with different App IDs cannot call each other even if they join the same channel.

## Next Steps

You are in the channel and can start a voice call with the following step:

- [Publish and Subscribe to Streams](../../en/Voice/publish_web_audio.md)

For more functions, you can refer to the following sections:

- [Adjust the Volume](../../en/Voice/volume_web.md)
- [Play Audio Effects/Audio Mixing](../../en/Voice/effect_mixing_web.md)
