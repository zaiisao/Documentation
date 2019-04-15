
---
title: Sending Point-to-point Text and Channel Messages from the Client
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 04:03:16 GMT+0800 (CST)
---
# Sending Point-to-point Text and Channel Messages from the Client
## Section 1: Integration

### Step 1: Download the Agora Signaling SDK.

Download the [Agora Signaling SDK](https://docs.agora.io/en/Agora%20Platform/downloads).

### Step 2: Unpack the downloaded SDK and save the corresponding files.

1.  Save the unpacked **libs/Dll/agorasdk.dll** to the **libs/Dll** folder of your local project.

2.  Save the unpacked **libs/include/agora\_api\_win.h** to the **libs/include** folder of your local project.

3.  Save the unpacked **libs/Lib/agorasdk.lib** to the **libs/Lib** folder of your local project.


### Step 3: Get an App ID and an App Certificate.

For more information see [App ID](../../en/Agora%20Platform/key_signaling.md).

### Step 4: Generate a token.

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
m_AgoraAPI = getAgoraSDKInstanceWin(m_AppId.data(), m_AppId.size());
```

```
/* Login to the Agora Signaling system. */
m_AgoraAPI->login2(gbk2utf8(m_AppId).data(), gbk2utf8(m_AppId).size(), gbk2utf8(m_Account).data(), gbk2utf8(m_Account).size(), gbk2utf8(m_channelKey).data(), gbk2utf8(m_channelKey).size(), 0, "", 0, retryTime, retryCount);
```

```
/* Set the onLoginSuccess callback. */
void CSingleCallBack::onLoginSuccess(uint32_t uid, int fd){
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
void CSingleCallBack::onLoginFailed(int ecode){
     /* Your code. */
}
```

```
/* Send a point-to-point message. */
m_AgoraAPI->messageInstantSend(gbk2utf8(account).data(), gbk2utf8(account).size(), 0, gbk2utf8(instanmsg).data(), gbk2utf8(instanmsg).size(), gbk2utf8(msgId).data(), gbk2utf8(msgId).size());
```

```
 /* Send a point-to-point message. */
void CSingleCallBack::onMessageSendSuccess(char const * messageID, size_t messageID_size){
       /* Your code. */
}
```

```
/* Set the onMessageSendSuccess callback. */
void CSingleCallBack::onMessageSendError(char const * messageID, size_t messageID_size, int ecode){
       /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
m_AgoraAPI->logout();
```

#### Receiver

```
/* Initialize the Signaling SDK. */
m_AgoraAPI = getAgoraSDKInstanceWin(m_AppId.data(), m_AppId.size());
```

```
/* Login to the Agora Signaling system. */
m_AgoraAPI->login2(gbk2utf8(m_AppId).data(), gbk2utf8(m_AppId).size(), gbk2utf8(m_Account).data(), gbk2utf8(m_Account).size(), gbk2utf8(m_channelKey).data(), gbk2utf8(m_channelKey).size(), 0, "", 0, retryTime, retryCount);
```

```
/* Set the onLoginSuccess callback. */
void CSingleCallBack::onLoginSuccess(uint32_t uid, int fd){
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
void CSingleCallBack::onLoginFailed(int ecode){
     /* Your code. */
}
```

```
 /* Set the onMessageInstantReceive callback. */
 void CSingleCallBack::onMessageInstantReceive(char const * account, size_t account_size, uint32_t uid, char const * msg, size_t msg_size){
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
m_AgoraAPI->logout();
```

### Sending a Channel Message

#### Sender

```
/* Initialize the Signaling SDK. */
m_AgoraAPI = getAgoraSDKInstanceWin(m_AppId.data(), m_AppId.size());
```

```
/* Login to the Agora Signaling system. */
m_AgoraAPI->login2(gbk2utf8(m_AppId).data(), gbk2utf8(m_AppId).size(), gbk2utf8(m_Account).data(), gbk2utf8(m_Account).size(), gbk2utf8(m_channelKey).data(), gbk2utf8(m_channelKey).size(), 0, "", 0, retryTime, retryCount);
```

```
/* Set the onLoginSuccess callback. */
void CSingleCallBack::onLoginSuccess(uint32_t uid, int fd){
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
void CSingleCallBack::onLoginFailed(int ecode){
     /* Your code. */
}
```

```
/* Join a channel. */
agora->channelJoin(g_channel.c_str(), g_channel.size())
```

```
/* Set the onChannelJoined callback. */
virtual void onChannelJoined(char const * channelID, size_t channelID_size)  override{
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
virtual void onChannelJoinFailed(char const * channelID, size_t channelID_size,int ecode)  override{
      /* Your code. */
}
```

```
/* Send a channel message. */
m_AgoraAPI->messageChannelSend(gbk2utf8(channel).data(), gbk2utf8(channel).size(), gbk2utf8(ChannelMsg).data(), gbk2utf8(ChannelMsg).size(), gbk2utf8(msgId).data(), gbk2utf8(msgId).size());
```

```
/* Set the onMessageSendSuccess callback. */
void CSingleCallBack::onMessageSendSuccess(char const * messageID, size_t messageID_size){
       /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
void CSingleCallBack::onMessageSendError(char const * messageID, size_t messageID_size, int ecode){
       /* Your code. */
}
```

```
/* Leave the channel. */
agora->channelleave(g_channel.c_str(), g_channel.size())
```

```
/* Set the onChannelLeaved callback. */
virtual void onChannelLeaved(char const * channelID, size_t channelID_size,int ecode) {
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
m_AgoraAPI->logout();
```

#### Receiver

```
/* Initialize the Signaling SDK. */
m_AgoraAPI = getAgoraSDKInstanceWin(m_AppId.data(), m_AppId.size());
```

```
/* Login to the Agora Signaling system. */
m_AgoraAPI->login2(gbk2utf8(m_AppId).data(), gbk2utf8(m_AppId).size(), gbk2utf8(m_Account).data(), gbk2utf8(m_Account).size(), gbk2utf8(m_channelKey).data(), gbk2utf8(m_channelKey).size(), 0, "", 0, retryTime, retryCount);
```

```
/* Set the onLoginSuccess callback. */
void CSingleCallBack::onLoginSuccess(uint32_t uid, int fd){
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
void CSingleCallBack::onLoginFailed(int ecode){
     /* Your code. */
}
```

```
/* Join a channel. */
agora->channelJoin(g_channel.c_str(), g_channel.size())
```

```
/* Set the onChannelJoined callback. */
virtual void onChannelJoined(char const * channelID, size_t channelID_size)  override{
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
virtual void onChannelJoinFailed(char const * channelID, size_t channelID_size,int ecode)  override{
      /* Your code. */
}
```

```
/* Set the onMessageChannelReceive callback. */
void CSingleCallBack::onMessageChannelReceive(char const * channelID, size_t channelID_size, char const * account, size_t account_size, uint32_t uid, char const * msg, size_t msg_size){
      /* Your code. */
}
```

```
/* Leave the channel. */
agora->channelleave(g_channel.c_str(), g_channel.size())
```

```
/* Set the onChannelLeaved callback. */
virtual void onChannelLeaved(char const * channelID, size_t channelID_size,int ecode) {
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
m_AgoraAPI->logout();
```


