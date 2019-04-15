
---
title: Sending Point-to-point Text and Channel Messages from the Client
description: 
platform: Web
updatedAt: Fri Nov 02 2018 04:03:08 GMT+0800 (CST)
---
# Sending Point-to-point Text and Channel Messages from the Client
## Section 1: Integration

### Step 1: Download the Agora Signaling SDK.

Download the [Agora Signaling SDK](https://docs.agora.io/en/Agora%20Platform/downloads).

### Step 2: Unpack the downloaded SDK.

Save the unpacked **SDK** to the **web/src/assets/vendor/** folder of your local project.

### Step 3: Get an App ID and an App Certificate.

For more information see [App ID](../../en/Agora%20Platform/key_signaling.md).

### Step 4: Generate a token.

Agora recommends generating a token on the server. For more information about generating a token, see: [signalingToken](../../en/Agora%20Platform/key_signaling.md).

You are now all set and can call the APIs provided by the Agora Signaling SDK.

## Section 2: Scenario-based Code Snippets

This section provides code snippets related to the following two scenarios:

-   Sending point-to-point messages

-   Sending channel messages


### Sending a Point-to-point Message

```
 /* Login to the Agora Signaling System. */
 var session = signal.login(account, token);
 session.onLoginSuccess = function(uid){
/* Join a channel. */
session.onMessageInstantReceive = function(account, uid, msg){
/* Set the onMessageInstantReceive callback. */
};
/* Send a point-to-point message. */
session.messageInstantSend(receiver, msg);

/* Logout of the system. */
session.logout();
};

session.onLogout = function(ecode){
/* Set the onLogout callback. */
}
```

### Sending a Channel Message

```
/* Login to the system. */
var session = signal.login(account, token);
session.onLoginSuccess = function(uid){
/* Join a channel and set the onChannelJoined and onMessagChannelReceive callbacks. */
var channel = session.channelJoin(channelname);
channel.onChannelJoined = function(){
channel.onMessageChannelReceive = function(account, uid, msg){

}
/* Send a channel message. */
channel.messageChannelSend(text);

/* Logout of the system. */
session.logout();
 };
};

session.onLogout = function(ecode){
/* Set the onLogout callback. */
}
```


