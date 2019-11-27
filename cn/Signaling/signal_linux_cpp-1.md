
---
title: 客户端：发送点对点文本消息和频道文本消息
description: 
platform: Linux
updatedAt: Mon Nov 18 2019 06:42:14 GMT+0800 (CST)
---
# 客户端：发送点对点文本消息和频道文本消息
## 第一节：快速集成

本章介绍信令 SDK 的集成方法。

### 第一步：

准备开发环境。

### 第二步：下载最新的 Agora Signaling SDK 软件包。

下载地址： [Agora Signaling SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads)。

### 第三步：

解压已下载的 SDK 软件包。

### 第四步：复制解压包中的 libs 文件夹。

1.  下载 [Agora Signaling SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads)。

2.  将解压后的 **libs** 文件夹内的 `libagorasig.so` 库复制到您本地项目的 **libs** 文件夹下。

3.  将解压后的 **include** 文件夹内的 `agora\_sig.h` 头文件复制到您本地项目的 **include** 文件夹下。


### 第五步：获取 App ID 和 App 证书。

详见 [SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

### 第六步：计算 token

建议您在服务器端计算 token 动态秘钥。token 计算方法详见：[SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

您现在已经准备就绪，可以调用 Agora Signaling SDK 提供的 API 了。

## 第二节：信令相关场景介绍及相关代码

本节提供如下两种场景的代码片段：

-   发送点对点消息

-   发送频道消息


### 发送点对点消息

以下为发送点对点消息相关的示例代码。

#### 发送端

```
// 初始化信令 SDK
agora = getAgoraSDKInstanceCPP();
```

```
// 登录 Agora 信令系统
agora->login(g_vendor.data(),g_vendor.size(),g_username.data(),g_username.size(),g_token.data(),g_token.size(),g_uid,"",0);
```

```
// 设置登录成功回调
virtual void onLoginSuccess(uint32_t uid, int fd) override {
       // Your code
}
```

```
// 设置登录失败回调
virtual void onLoginFailed(int ecode)  override{
       // Your code
}
```

```
// 发送点对点消息
agora->messageInstantSend(account.c_str(), account.size(), 0, msg.data(), msg.size(),msgID.data(), msgID.size());
```

```
// 设置消息发送成功回调
virtual void onMessageSendSuccess(char const * messageID, size_t messageID_size) {
       // Your code
}
```

```
// 设置消息发送失败回调
virtual void onMessageSendError(char const * messageID, size_t messageID_size,int ecode) {
       // Your code
}
```

```
// 退出 Agora 信令系统
agora->logout();
```

#### 接收端（对端）

```
// 初始化信令 SDK
agora = getAgoraSDKInstanceCPP();
```

```
// 登录 Agora 信令系统
agora->login(g_vendor.data(),g_vendor.size(),g_username.data(),g_username.size(),g_token.data(),g_token.size(),g_uid,"",0);
```

```
// 设置登录成功回调
virtual void onLoginSuccess(uint32_t uid, int fd) override {
      // Your code
}
```

```
// 设置登录失败回调
virtual void onLoginFailed(int ecode)  override{
      // Your code
}
```

```
 // 设置对端收到消息回调
 virtual void onMessageInstantReceive(char const * account, size_t account_size,uint32_t uid,char const * msg, size_t msg_size)  override{


      // Your code
}
```

```
// 退出 Agora 信令系统
agora->logout();
```

### 发送频道消息

以下为发送频道消息相关的示例代码。

#### 发送端

```
// 初始化信令 SDK
agora = getAgoraSDKInstanceCPP();
```

```
// 登录 Agora 信令系统
agora->login(g_vendor.data(),g_vendor.size(),g_username.data(),g_username.size(),g_token.data(),g_token.size(),g_uid,"",0);
```

```
// 设置登录成功回调
virtual void onLoginSuccess(uint32_t uid, int fd) override {
      // Your code
}
```

```
// 设置登录失败回调
virtual void onLoginFailed(int ecode)  override{
      // Your code
}
```

```
// 加入频道
agora->channelJoin(g_channel.c_str(), g_channel.size())
```

```
// 设置加入频道成功回调
virtual void onChannelJoined(char const * channelID, size_t channelID_size)  override{
     // Your code
}
```

```
// 设置加入频道失败回调
virtual void onChannelJoinFailed(char const * channelID, size_t channelID_size,int ecode)  override{
      // Your code
}
```

```
// 发送频道消息
agora->messageChannelSend(g_channel.c_str(), g_channel.size(),msg.data(), msg.size(),msgID.data(), msgID.size());
```

```
// 设置消息发送成功回调
virtual void onMessageSendSuccess(char const * messageID, size_t messageID_size) {
      // Your code
}
```

```
// 设置消息发送失败回调
virtual void onMessageSendError(char const * messageID, size_t messageID_size,int ecode) {
      // Your code
}
```

```
// 退出频道
agora->channelleave(g_channel.c_str(), g_channel.size())
```

```
// 设置退出频道回调
virtual void onChannelLeaved(char const * channelID, size_t channelID_size,int ecode) {
      // Your code
}
```

```
// 退出 Agora 信令系统
agora->logout()
```

#### 接收端 （对端）

```
// 初始化信令 SDK
agora = getAgoraSDKInstanceCPP();
```

```
// 登录 Agora 信令系统
agora->login(g_vendor.data(),g_vendor.size(),g_username.data(),g_username.size(),g_token.data(),g_token.size(),g_uid,"",0);
```

```
// 设置登录成功回调
virtual void onLoginSuccess(uint32_t uid, int fd) override {
      // Your code
}
```

```
// 设置登录失败回调
virtual void onLoginFailed(int ecode)  override{
      // Your code
}
```

```
// 加入频道
agora->channelJoin(g_channel.c_str(), g_channel.size())
```

```
// 设置加入频道成功回调
virtual void onChannelJoined(char const * channelID, size_t channelID_size)  override{
     // Your code
}
```

```
// 设置加入频道失败回调
virtual void onChannelJoinFailed(char const * channelID, size_t channelID_size,int ecode)  override{
      // Your code
}
```

```
// 设置对端接收到频道消息回调
 virtual void onMessageChannelReceive(char const * channelID, size_t channelID_size,char const * account, size_t account_size,uint32_t uid,char const * msg, size_t msg_size)  override{
      // Your code
}
```

```
// 退出频道
agora->channelleave(g_channel.c_str(), g_channel.size())
```

```
// 设置退出频道回调
virtual void onChannelLeaved(char const * channelID, size_t channelID_size,int ecode) {
      // Your code
}
```

```
// 退出 Agora 信令系统
agora->logout()
```


