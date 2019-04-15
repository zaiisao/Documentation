
---
title: Sending Point-to-point Text and Channel Messages from the Client
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:02:29 GMT+0800 (CST)
---
# Sending Point-to-point Text and Channel Messages from the Client
## Section 1: Integration

### Step 1: Prepare the Environment.

Ensure that your development environment meets the following requirements:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>OS</strong></td>
<td><strong>System Requirements</strong></td>
<td><strong>Network Requirements</strong></td>
</tr>
<tr><td>Android</td>
<td>Android API 15 or later</td>
<td>HTTP Port 80. TCP Port 8181</td>
</tr>
</tbody>
</table>



> Use a real Android device or emulator.

### Step 2: Download the Agora Signaling SDK and Unpack the SDK.

Download the SDK from [Agora Signaling SDK](https://docs.agora.io/en/Agora%20Platform/downloads).

### Step 3: Save the corresponding files.

1.  Copy the `.jar` file under the **libs** folder, and save it to the **app/libs** folder of your project.

2.  Copy the `.so` files under **libs/arm64-v8a/x86/armeabi-v7a** to the **app/src/main/libs** folder of your project.


### Step 4: Set the Android attribute.

In your project, add the following code snippet to the Android attribute of the `app/build.gradle` file.

```
sourceSets {
     main {
     jniLibs.srcDirs = ['src/main/libs']
          }
       }
```

### Step 5: Add the dependency.

Add the following dependency to the `app/build.gradle` file:

```
compile fileTree(include: ['*.jar'], dir: 'libs')
```

### Step 6: Synchronize the project.

Click **Sync Project with Gradle Files** to synchronize your project.

### Step 7: Get an App ID and App Certificate.

For more information see [App ID](../../en/Agora%20Platform/key_signaling.md).

### Step 8: Generate a token.

Agora recommends generating the token on the server. For more information about generating a token, see: [SignalingToken](../../en/Agora%20Platform/key_signaling.md).

You are now all set and can call the APIs provided by the Agora Signaling SDK.

## Section 2: Scenario-based Code Snippets

This section provides code snippets related to the following two scenarios:

-   Sending point-to-point messages

-   Sending channel messages


### Sending a Point-to-point Message

#### Sender

```
/* Initialize the SDK. */
m_agoraAPI = AgoraAPIOnlySignal.getInstance(context, appID);
```

```
/* Login to the Agora Signaling system. */
m_agoraAPI.login2(appId, account, token, uid, deviceID, retry_time_in_s, retry_count)
```

```
/* Set the onLoginSuccess callback. */
m_agoraAPI.onLoginSuccess(uid, fd) {
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
m_agoraAPI.onLoginFailed(ecode) {
      /* Your code. */
}
```

```
/* Send a point-to-point message. */
m_agoraAPI.messageInstantSend(account, uid, msg, msgID)
```

```
/* Set the onMessageSendSuccess callback. */
m_agoraAPI.onMessageSendSuccess(messageID){
     /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
m_agoraAPI.onMessageSendError(messageID, ecode) {
     /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
m_agoraAPI.logout()
```

#### Receiver

```
/* Initialize the Signaling SDK. */
m_agoraAPI = AgoraAPIOnlySignal.getInstance(context, appID);
```

```
/* Login to the Agora Signaling system. */
m_agoraAPI.login2(appId, account, token, uid, deviceID, retry_time_in_s, retry_count)
```

```
/* Set the onLoginSuccess callback. */
m_agoraAPI.onLoginSuccess(uid, fd) {
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
m_agoraAPI.onLoginFailed(ecode) {
      /* Your code. */
}
```

```
 /* Set the onMessageInstantReceive callback. */
 m_agoraAPI.onMessageInstantReceive(account, uid, msg){
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
m_agoraAPI.logout()
```

### Sending a Channel Message

#### Sender

```
/* Initialize the Signaling SDK. */
m_agoraAPI = AgoraAPIOnlySignal.getInstance(context, appID);
```

```
/* Login to the Agora Signaling system. */
m_agoraAPI.login2(appId, account, token, uid, deviceID, retry_time_in_s, retry_count)
```

```
/* Set the onLoginSuccess callback. */
m_agoraAPI.onLoginSuccess(uid,fd) {
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
m_agoraAPI.onLoginFailed(ecode){
      /* Your code. */
}
```

```
/* Join a channel. */
m_agoraAPI.channelJoin(channelName)
```

```
/* Set the onChannelJoined callback. */
m_agoraAPI.onChannelJoined(channelID){
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
m_agoraAPI.onChannelJoinFailed(channelID, ecode) {
      /* Your code. */
}
```

```
/* Send a channel message. */
m_agoraAPI.messageChannelSend(channelName, msg, msgID)
```

```
/* Set the onMessageSendSuccess callback. */
m_agoraAPI.onMessageSendSuccess(messageID) {
      /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
m_agoraAPI.onMessageSendError(messageID, ecode){
      /* Your code. */
}
```

```
/* Leave the channel. */
m_agoraAPI.channelLeave(channelName)
```

```
/* Set the onChannelLeaved callback. */
m_agoraAPI.onChannelLeaved(channelID, ecode) {
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
m_agoraAPI.logout()
```

#### Receiver

```
/* Initialize the Signaling SDK. */
m_agoraAPI = AgoraAPIOnlySignal.getInstance(context, appID);
```

```
/* Login to the Agora Signaling system. */
m_agoraAPI.login2(appId, account, token, uid, deviceID, retry_time_in_s, retry_count)
```

```
/* Set the onLoginSuccess callback. */
m_agoraAPI.onLoginSuccess(uid,fd) {
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
m_agoraAPI.onLoginFailed(ecode) {
      /* Your code. */
}
```

```
/* Join a channel. */
m_agoraAPI.channelJoin(channelName)
```

```
/* Set the onChannelJoined callback. */
m_agoraAPI.onChannelJoined(channelID) {
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
m_agoraAPI.onChannelJoinFailed(channelID, ecode) {
      /* Your code. */
}
```

```
/* Set the onMessageChannelReceive callback. */
m_agoraAPI.onMessageChannelReceive(channelID, account, uid, msg) {
      /* Your code. */
}
```

```
/* Leave the channel. */
m_agoraAPI.channelLeave(channelName)
```

```
/* Set the onChannelLeaved callback. */
m_agoraAPI.onChannelLeaved(channelID, ecode) {
      /* Your code. */
}
```

```
/* Leave the Agora Signaling system. */
m_agoraAPI.logout()
```


