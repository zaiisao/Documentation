
---
title: Sending Point-to-point Text and Channel Messages from the Client
description: 
platform: Linux
updatedAt: Fri Nov 02 2018 04:02:44 GMT+0800 (CST)
---
# Sending Point-to-point Text and Channel Messages from the Client
## Section 1: Integration

### Step 1: Download the Agora Signaling SDK and Unpack the SDK.

Download the SDK from [Agora Signaling SDK](https://docs.agora.io/en/Agora%20Platform/downloads).

### Step 2: Save the corresponding files.

1.  Save **libagorasig.so** under the **libs** folder of the unpacked SDK to the **libs** folder of your local project.

2.  Save the **agora\_sig.h** header file under the **include** folder to the **include** folder of your local project.


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
// Initialize the SDK
agora = getAgoraSDKInstanceCPP();
```

```
// Login to the Agora Signaling system.
agora->login(g_vendor.data(),g_vendor.size(),g_username.data(),g_username.size(),g_token.data(),g_token.size(),g_uid,"",0);
```

```
// Set the onLoginSuccess callback.
virtual void onLoginSuccess(uint32_t uid, int fd) override {
       // Your code
}
```

```
// Set the onLoginFailed callback.
virtual void onLoginFailed(int ecode)  override{
       // Your code
}
```

```
// Send a point-to-point message.
agora->messageInstantSend(account.c_str(), account.size(), 0, msg.data(), msg.size(),msgID.data(), msgID.size());
```

```
// Set the onMessageSendSuccess callback.
virtual void onMessageSendSuccess(char const * messageID, size_t messageID_size) {
       // Your code
}
```

```
// Set the onMessageSendError callback.
virtual void onMessageSendError(char const * messageID, size_t messageID_size,int ecode) {
       // Your code
}
```

```
// Logout of the Agora Signaling system.
agora->logout();
```

#### Receiver

```
// Initialize the Signaling SDK.
agora = getAgoraSDKInstanceCPP();
```

```
// Login to the Agora Signaling system.
agora->login(g_vendor.data(),g_vendor.size(),g_username.data(),g_username.size(),g_token.data(),g_token.size(),g_uid,"",0);
```

```
// Set the onLoginSuccess callback
virtual void onLoginSuccess(uint32_t uid, int fd) override {
      // Your code
}
```

```
// Set the onLoginFailed callback
virtual void onLoginFailed(int ecode)  override{
      // Your code
}
```

```
 // Set the onMessageInstantReceive callback.
 virtual void onMessageInstantReceive(char const * account, size_t account_size,uint32_t uid,char const * msg, size_t msg_size)  override{


      // Your code
}
```

```
// Logout of the Agora Signaling system.
agora->logout();
```

### Sending a Channel Message

#### Sender

```
// Initialize the Signaling SDK
agora = getAgoraSDKInstanceCPP();
```

```
// Login to the Agora Signaling system
agora->login(g_vendor.data(),g_vendor.size(),g_username.data(),g_username.size(),g_token.data(),g_token.size(),g_uid,"",0);
```

```
// Set the onLoginSuccess callback.
virtual void onLoginSuccess(uint32_t uid, int fd) override {
      // Your code
}
```

```
// Set the onLoginFailed callback.
virtual void onLoginFailed(int ecode)  override{
      // Your code
}
```

```
// Join a channel.
agora->channelJoin(g_channel.c_str(), g_channel.size())
```

```
// Set the onChannelJoined callback.
virtual void onChannelJoined(char const * channelID, size_t channelID_size)  override{
     // Your code
}
```

```
// Set the onChannelJoinFailed callback.
virtual void onChannelJoinFailed(char const * channelID, size_t channelID_size,int ecode)  override{
      // Your code
}
```

```
// Send a channel message.
agora->messageChannelSend(g_channel.c_str(), g_channel.size(),msg.data(), msg.size(),msgID.data(), msgID.size());
```

```
// Set the onMessageSendSuccess callback.
virtual void onMessageSendSuccess(char const * messageID, size_t messageID_size) {
      // Your code
}
```

```
// Set the onMessageSendError callback.
virtual void onMessageSendError(char const * messageID, size_t messageID_size,int ecode) {
      // Your code
}
```

```
// Leave the channel.
agora->channelleave(g_channel.c_str(), g_channel.size())
```

```
// Set the onChannelLeaved callback.
virtual void onChannelLeaved(char const * channelID, size_t channelID_size,int ecode) {
      // Your code
}
```

```
// Logout of the Agora Signaling system.
agora->logout()
```

#### Receiver

```
// Initialize the Signaling SDK
agora = getAgoraSDKInstanceCPP();
```

```
// Login to the Agora Signaling system.
agora->login(g_vendor.data(),g_vendor.size(),g_username.data(),g_username.size(),g_token.data(),g_token.size(),g_uid,"",0);
```

```
// Set the onLoginSuccess callback.
virtual void onLoginSuccess(uint32_t uid, int fd) override {
      // Your code
}
```

```
// Set the onLoginFailed callback.
virtual void onLoginFailed(int ecode)  override{
      // Your code
}
```

```
// Join a channel.
agora->channelJoin(g_channel.c_str(), g_channel.size())
```

```
// Set the onChannelJoined callback.
virtual void onChannelJoined(char const * channelID, size_t channelID_size)  override{
     // Your code
}
```

```
// Set the onChannelJoinFailed callback.
virtual void onChannelJoinFailed(char const * channelID, size_t channelID_size,int ecode)  override{
      // Your code
}
```

```
// Set the onMessageChannelReceive callback.
 virtual void onMessageChannelReceive(char const * channelID, size_t channelID_size,char const * account, size_t account_size,uint32_t uid,char const * msg, size_t msg_size)  override{
      // Your code
}
```

```
// Leave the channel.
agora->channelleave(g_channel.c_str(), g_channel.size())
```

```
// Set the onChannelLeaved callback.
virtual void onChannelLeaved(char const * channelID, size_t channelID_size,int ecode) {
      // Your code
}
```

```
// Leave the Agora Signaling system.
agora->logout()
```


