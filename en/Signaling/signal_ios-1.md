
---
title: Sending Point-to-point Text and Channel Messages from the Client
description: 
platform: iOS
updatedAt: Fri Nov 02 2018 04:02:36 GMT+0800 (CST)
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
<tr><td>iOS</td>
<td>iOS 7.0 or later</td>
<td>HTTP Port 80. TCP Port 8181.</td>
</tr>
</tbody>
</table>


### Step 2: Download the Agora Signaling SDK.

Download the [Agora Signaling SDK](https://docs.agora.io/en/Agora%20Platform/downloads).

### Step 3: Unpack the downloaded SDK and save the files in the libs folder to the corresponding folder of your project.

> The library provided by the Agora Signaling SDK is a FAT image,which includes versions for the 32/64-bit emulator and the 32/64-bit real device.

### Step 4: Add the required framework.

To add the required library:

1.  Choose the current **Target**.

2.  In Xcode, set the search path of the framework to the **Agora SDK lib** folder.

3.  Click **Building Phases** to add the required library.


<img alt="../_images/xcode_4.png" src="https://web-cdn.agora.io/docs-files/en/xcode_4.png" style="width: 992.0px; height: 299.0px;"/>


1.  Click **Link Binary with Libraries** \> **+** to add the following libraries.

    <img alt="Quickstart Guide/ios_project_3.png" src="https://web-cdn.agora.io/docs-files/en/ios_project_3.png"/>



> `AgoraSigKit.framework` is in the **libs** folder of your project. Click **+** > **Add Other…** to enter the project folder to add the libraries.

<br/>

<img alt="../_images/xcode_5.png" src="https://web-cdn.agora.io/docs-files/en/xcode_5.png" style="width: 993.0px; height: 512.0px;"/>


### Step 5: Miscellaneous settings.

1.  Choose the current **Target**.

2.  Enable or disable **Bitcode** according to your actual needs:

    <img alt="../_images/bitcode.png" src="https://web-cdn.agora.io/docs-files/en/bitcode.png" style="width: 1172.4px; height: 400.8px;"/>



### Step 6: Get an App ID and an App Certificate.

For more information see [App ID](../../en/Agora%20Platform/key_signaling.md).

### Step 7: Generate a token.

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
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
/* Login to the Agora Signaling system. */
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
/* Set the onLoginSuccess callback. */
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      /* Your code. */
}
```

```
/* Send a point-to-point message. */
AgoraSignalKit.messageInstantSend(account, uid: 0, msg: message, msgID: msgID)
```

```
/* Set the onMessageSendSuccess callback. */
AgoraSignalKit.onMessageSendSuccess = { (messageID) -> () in
     /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
AgoraSignalKit.onMessageSendError = { (messageID, ecode) -> () in
     /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
AgoraSignalKit.logout()
```

#### Receiver

```
/* Initialize the Signaling SDK. */
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
/* Login to the Agora Signaling system. */
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
/* Set the onLoginSuccess callback. */
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
     /* Your code. */
}
```

```
 /* Set the onMessageInstantReceive callback. */
 AgoraSignalKit.onMessageInstantReceive = { (account, uid, msg) -> () in
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
AgoraSignalKit.logout()
```

### Sending a Channel Message

#### Sender

```
/* Initialize the Signaling SDK. */
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
/* Login to the Agora Signaling system. */
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
/* Set the onLoginSuccess callback. */
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      /* Your code. */
}
```

```
/* Join a channel. */
AgoraSignalKit.channelJoin(channelName)
```

```
/* Set the onChannelJoined callback. */
AgoraSignalKit.onChannelJoined = { (channelID) -> () in
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
AgoraSignalKit.onChannelJoinFailed = { (channelID, ecode) -> () in
      /* Your code. */
}
```

```
/* Send a channel message. */
AgoraSignalKit.messageChannelSend(channelName, msg: message, msgID: msgID)
```

```
/* Set the onMessageSendSuccess callback. */
AgoraSignalKit.onMessageSendSuccess = { (messageID) -> () in
      /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
AgoraSignalKit.onMessageSendError = { (messageID, ecode) -> () in
      /* Your code. */
}
```

```
/* Leave the channel. */
AgoraSignalKit.channelLeave(channelName)
```

```
/* Set the onChannelLeaved callback. */
AgoraSignalKit.onChannelLeaved = { (channelID, ecode) -> () in
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
AgoraSignalKit.logout()
```

#### Receiver

```
/* Initialize the Signaling SDK. */
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
/* Login to the Agora Signaling system. */
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
/* Set the onLoginSuccess callback. */
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      /* Your code. */
}
```

```
/* Join a channel. */
AgoraSignalKit.channelJoin(channelName)
```

```
/* Set the onChannelJoined callback. */
AgoraSignalKit.onChannelJoined = { (channelID) -> () in
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
AgoraSignalKit.onChannelJoinFailed = { (channelID, ecode) -> () in
      /* Your code. */
}
```

```
/* Set the onMessageChannelReceive callback. */
AgoraSignalKit.onMessageChannelReceive = { (channelID, account, uid, msg) -> () in
      /* Your code. */
}
```

```
/* Leave the channel. */
AgoraSignalKit.channelLeave(channelName)
```

```
/* Set the onChannelLeaved callback. */
AgoraSignalKit.onChannelLeaved = { (channelID, ecode) -> () in
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
AgoraSignalKit.logout()
```


