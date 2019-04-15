
---
title: Sending Point-to-point Text and Channel Messages from the Server
description: 
platform: Java
updatedAt: Fri Nov 02 2018 04:03:26 GMT+0800 (CST)
---
# Sending Point-to-point Text and Channel Messages from the Server
## Section 1: Integration

### Step 1: Prepare the Environment

Ensure that your development environment meets the following requirements:

-   Java 1.5 or later

-   IDE that supports Gradle


### Step 2: Download the Agora Signaling SDK and Unpack the SDK.

Download the SDK from [Agora Signaling SDK](https://docs.agora.io/en/Agora%20Platform/downloads).

### Step 3: Save the files in the **libs** folder to the corresponding folder of your project.

Save all the `.jar` files under the **libs** and **libs-dep** folders to the **libs** folder of your local project.

### Step 4: Get an App ID and an App Certificate.

For more information see [App ID](../../en/Agora%20Platform/key_signaling.md).

### Step 5: Generate a token.

Agora recommends generating a token on the server. For more information about generating a token, see: [SignalingToken](../../en/Agora%20Platform/key_signaling.md).

You are now all set and can call the APIs provided by the Agora Signaling SDK.

## Section 2: Scenario-based Code Snippets

This section provides code snippets related to the following two scenarios:

-   Sending point-to-point messages

-   Sending channel messages


### Sending a Point-to-point Message

#### Sender

```
/* Initialize the SDK. */
Signal signal = new Signal(appId);
```

```
/* Login to the Agora Signaling system. */
signal.login(accountName, this.token, new Signal.LoginCallback()）
```

```
/* Set the onLoginSuccess callback. */
public void onLoginSuccess(final Signal.LoginSession session, int uid)  {
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
public void onLoginFailed(LoginSession session, int ecode)  {
      /* Your code. */
}
```

```
/* Send a point-to-point message. */
Signal.LoginSession currentSession = users.get(currentUser).getSession();
currentSession.messageInstantSend(oppositeAccount, msg, new Signal.MessageCallback()
```

```
/* Set the onMessageSendSuccess callback. */
public void onMessageSendSuccess(Signal.LoginSession session) {
     /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
public void onMessageSendError(Signal.LoginSession session, int ecode)  {
     /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
currentSession.logout()
```

#### Receiver

```
/* Initialize the Signaling SDK. */
Signal signal = new Signal(appId);
```

```
/* Login to the Agora Signaling system. */
signal.login(accountName, this.token, new Signal.LoginCallback()）
```

```
/* Set the onLoginSuccess callback. */
public void onLoginSuccess(final Signal.LoginSession session, int uid) {
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
public void onLoginFailed(LoginSession session, int ecode)  {
      /* Your code. */
}
```

```
 /* Set the onMessageInstantReceive callback. */
 public void onMessageInstantReceive(Signal.LoginSession session, String account, int uid, String msg) {
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
currentSession.logout()
```

### Sending a Channel Message

#### Sender

```
/* Initialize the Signaling SDK. */
Signal signal = new Signal(appId);
```

```
/* Login to the Agora Signaling system. */
signal.login(accountName, this.token, new Signal.LoginCallback()）
```

```
/* Set the onLoginSuccess callback. */
public void onLoginSuccess(final Signal.LoginSession session, int uid)  {
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
public void onLoginFailed(LoginSession session, int ecode) {
      /* Your code. */
}
```

```
/* Join a channel. */
final CountDownLatch channelJoindLatch = new CountDownLatch(1);
Channel channel = users.get(currentUser).getSession().channelJoin(channelName, new Signal.ChannelCallback()
```

```
/* Set the onChannelJoined callback. */
public void onChannelJoined(Signal.LoginSession session, Signal.LoginSession.Channel channelName) {
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
public void onChannelJoinFailed(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode) {
      /* Your code. */
}
```

```
/* Send a channel message. */
users.get(currentUser).getChannel().messageChannelSend(msg)
```

```
/* Set the onMessageSendSuccess callback. */
public void onMessageSendSuccess(Signal.LoginSession session) {
     /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
public void onMessageSendError(Signal.LoginSession session, int ecode)  {
     /* Your code. */
}
```

```
/* Leave the channel. */
users.get(currentUser).getChannel().channelLeave()
```

```
/* Set the onChannelLeaved callback. */
public void onChannelLeaved(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode) {
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
currentSession.logout()
```

#### Receiver

```
/* Initialize the Signaling SDK. */
Signal signal = new Signal(appId);
```

```
/* Login to the Agora Signaling system. */
signal.login(accountName, this.token, new Signal.LoginCallback()
```

```
/* Set the onLoginSuccess callback. */
public void onLoginSuccess(final Signal.LoginSession session, int uid) {
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
public void onLoginFailed(LoginSession session, int ecode)  {
      /* Your code. */
}
```

```
/* Join a channel. */
final CountDownLatch channelJoindLatch = new CountDownLatch(1);
Channel channel = users.get(currentUser).getSession().channelJoin(channelName, new Signal.ChannelCallback()
```

```
/* Set the onChannelJoined callback. */
public void onChannelJoined(Signal.LoginSession session, Signal.LoginSession.Channel channelName)  {
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
public void onChannelJoinFailed(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode) {
      /* Your code. */
}
```

```
/* Set the onMessageChannelReceive callback. */
public void onMessageChannelReceive(Signal.LoginSession session, Signal.LoginSession.Channel channel, String account, int uid, String msg) {
      /* Your code. */
}
```

```
/* Leave the channel. */
users.get(currentUser).getChannel().channelLeave()
```

```
/* Set the onChannelLeaved callback. */
public void onChannelLeaved(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode) {
      /* Your code. */
}
```

```
/* Leave the Agora Signaling system. */
currentSession.logout()
```


