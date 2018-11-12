
---
title: Join a Channel
description: 
platform: Web
updatedAt: Fri Nov 02 2018 04:08:11 GMT+0000 (UTC)
---
# Join a Channel
Once the client initialization is complete, call the  `client.join`  method in the `onSuccess` callback.

Pass the channel key, channel name, and user ID to the method parameters:

- `tokenOrKey`: For low-security requirements, pass null as the parameter value. For high-security requirements, pass the string of the token or Channel Key as the parameter value. See [Security Keys](../../en/Voice/token.md).
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
