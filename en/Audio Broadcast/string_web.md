
---
title: Use String User Accounts
description: 
platform: Web
updatedAt: Thu Sep 19 2019 06:44:07 GMT+0800 (CST)
---
# Use String User Accounts
## Introduction

Many apps use string usernames. To reduce development costs, Agora adds support for string user accounts. Users can now directly use their string usernames as user accounts to join the Agora channel.

To ensure smooth communication, all the users in a channel should use the same type of user account, that is, either the integer user ID, or the string user account.


## Implementation

Ensure that you understand the steps and code logic for implmenting the basic real-time communication functions. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_web.md) or [Start a live broadcast](../../en/Audio%20Broadcast/start_live_web.md).

The `uid` parameter in the `Client.join` method can be set as either a number or a string. You can join a channel by calling the `Client.join` method and passing in a string `uid`.


### API call sequence

The following diagram shows how to join a channel with a string user account:

![](https://web-cdn.agora.io/docs-files/1568875442230)

### Sample code

You can also refer to the following code snippets and implement string user accounts in your peoject:

```javascript
// Set uid as agora and join channel 1024
client.join("<token>", "1024", "agora", function(uid) {
  console.log("client" + uid + "joined channel");
  // Create a local stream
  // ...
}, function(err) {
  console.error("client join failed", err)
  // Error handling
});
```

In which, "agora" is a string user ID.

### API Reference
* [`Client.join`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#join)

## Considerations

- Do not mix parameter types within the same channel. If you use SDKs that do not support string usernames, only integer user IDs can be used in the channel. The following Agora SDKs support string user accounts:
  - The Native SDK: v2.8.0 and later.
  - The Web SDK: v2.5.0 and later.
- If you change your app usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts to join a channel, ensure that the token generation script on your server is updated to the latest version, and that you use the same user account or its corresponding integer user ID to generate a token. 
- If the Native SDK and Web SDK join the same channel, ensure that the user identification types are the same. 
