
---
title: Join a Channel
description: 
platform: Web
updatedAt: Thu Jul 18 2019 11:45:00 GMT+0800 (CST)
---
# Join a Channel
## Prerequisites

Before joining a channel, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/web_prepare.md).

When working on a test version of your application, you can generate a temporary token at the [Agora Dashboard](https://dashboard.agora.io/) to join a channel. 

On the **Project Details** page, click **Generate a Temp Token**, enter a channel name, and you will get a temporary token on the **Token** page. 

![](https://web-cdn.agora.io/docs-files/1563113619615)

![](https://web-cdn.agora.io/docs-files/1563113643411)

> - Ensure that you have enabled the App Certificate of the project before clicking **nerate a Temp Token** . See [Enablet the App Certificate](#appcertificate).
> - A temp token applies to scenarios with low security requirements. For the production environment, we recommend using a token generated at your server.

## Implementation

Once the client initialization is complete, call the `client.join`  method in the `onSuccess` callback.

Pass the channel key, channel name, and user ID to the method parameters:

- `tokenOrKey`: Pass a token that identifies the role and privilege of the user. A Temp Token can be used at the testing stage. For the production environment, we recommend using a Token generated at your server. For how to generate a token, see [Security Keys](../../en/Video/token.md). 
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

You are in the channel and can start a video call with the following step:

- [Publish and Subscribe to Streams](../../en/Video/publish_web.md)

For more functions, you can refer to the following sections:

- [Adjust the Volume](../../en/Video/volume_web.md)
- [Play Audio Effects/Audio Mixing](../../en/Video/effect_mixing_web.md)
- [Set the Video Profile](../../en/Video/videoProfile_web.md)
