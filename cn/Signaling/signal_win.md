
---
title: 客户端：发送点对点文本消息和频道文本消息
description: 
platform: Windows
updatedAt: Mon Nov 18 2019 06:42:16 GMT+0800 (CST)
---
# 客户端：发送点对点文本消息和频道文本消息
## 第一节：快速集成

本章介绍信令 SDK 的集成方法

### 第一步：

准备开发环境。

### 第二步：下载最新的 Agora Signaling SDK 软件包。

下载地址： [Agora Signaling SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads)。

### 第三步：

解压已下载的 SDK 软件包。

### 第四步：复制解压包中的 **libs** 文件夹。

1.  将解压后的 `libs/Dll/agorasdk.dll` 复制到您本地项目的 **libs/Dll** 文件夹下。

2.  将解压后的 `libs/include/agora_api_win.h` 复制到您本地项目的 **libs/include** 文件夹下。

3.  将解压后的 `libs/Lib/agorasdk.lib` 复制到您本地项目的 **libs/Lib** 文件夹下。


### 第五步：获取 App ID 和 App 证书。

详见 [SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

### 第六步：计算 token。

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
m_AgoraAPI = getAgoraSDKInstanceWin(m_AppId.data(), m_AppId.size());
```

```
// 登录 Agora 信令系统
m_AgoraAPI->login2(gbk2utf8(m_AppId).data(), gbk2utf8(m_AppId).size(), gbk2utf8(m_Account).data(), gbk2utf8(m_Account).size(), gbk2utf8(m_channelKey).data(), gbk2utf8(m_channelKey).size(), 0, "", 0, retryTime, retryCount);
```

```
// 设置登录成功回调
void CSingleCallBack::onLoginSuccess(uint32_t uid, int fd){
      // Your code
}
```

```
// 设置登录失败回调
void CSingleCallBack::onLoginFailed(int ecode){
     // Your code
}
```

```
// 发送点对点消息
m_AgoraAPI->messageInstantSend(gbk2utf8(account).data(), gbk2utf8(account).size(), 0, gbk2utf8(instanmsg).data(), gbk2utf8(instanmsg).size(), gbk2utf8(msgId).data(), gbk2utf8(msgId).size());
```

```
// 设置消息发送成功回调
void CSingleCallBack::onMessageSendSuccess(char const * messageID, size_t messageID_size){
       // Your code
}
```

```
// 设置消息发送失败回调
void CSingleCallBack::onMessageSendError(char const * messageID, size_t messageID_size, int ecode){
       // Your code
}
```

```
// 退出 Agora 信令系统
m_AgoraAPI->logout();
```

#### 接收端（对端）

```
// 初始化信令 SDK
m_AgoraAPI = getAgoraSDKInstanceWin(m_AppId.data(), m_AppId.size());
```

```
// 登录 Agora 信令系统
m_AgoraAPI->login2(gbk2utf8(m_AppId).data(), gbk2utf8(m_AppId).size(), gbk2utf8(m_Account).data(), gbk2utf8(m_Account).size(), gbk2utf8(m_channelKey).data(), gbk2utf8(m_channelKey).size(), 0, "", 0, retryTime, retryCount);
```

```
// 设置登录成功回调
void CSingleCallBack::onLoginSuccess(uint32_t uid, int fd){
      // Your code
}
```

```
// 设置登录失败回调
void CSingleCallBack::onLoginFailed(int ecode){
     // Your code
}
```

```
 // 设置对端收到消息回调
 void CSingleCallBack::onMessageInstantReceive(char const * account, size_t account_size, uint32_t uid, char const * msg, size_t msg_size){
      // Your code
}
```

```
// 退出 Agora 信令系统
m_AgoraAPI->logout();
```

### 发送频道消息

以下为发送频道消息相关的示例代码。

#### 发送端

```
// 初始化信令 SDK
m_AgoraAPI = getAgoraSDKInstanceWin(m_AppId.data(), m_AppId.size());
```

```
// 登录 Agora 信令系统
m_AgoraAPI->login2(gbk2utf8(m_AppId).data(), gbk2utf8(m_AppId).size(), gbk2utf8(m_Account).data(), gbk2utf8(m_Account).size(), gbk2utf8(m_channelKey).data(), gbk2utf8(m_channelKey).size(), 0, "", 0, retryTime, retryCount);
```

```
// 设置登录成功回调
void CSingleCallBack::onLoginSuccess(uint32_t uid, int fd){
      // Your code
}
```

```
// 设置登录失败回调
void CSingleCallBack::onLoginFailed(int ecode){
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
m_AgoraAPI->messageChannelSend(gbk2utf8(channel).data(), gbk2utf8(channel).size(), gbk2utf8(ChannelMsg).data(), gbk2utf8(ChannelMsg).size(), gbk2utf8(msgId).data(), gbk2utf8(msgId).size());
```

```
// 设置消息发送成功回调
void CSingleCallBack::onMessageSendSuccess(char const * messageID, size_t messageID_size){
       // Your code
}
```

```
// 设置消息发送失败回调
void CSingleCallBack::onMessageSendError(char const * messageID, size_t messageID_size, int ecode){
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
m_AgoraAPI->logout();
```

#### 接收端 （对端）

```
// 初始化信令 SDK
m_AgoraAPI = getAgoraSDKInstanceWin(m_AppId.data(), m_AppId.size());
```

```
// 登录 Agora 信令系统
m_AgoraAPI->login2(gbk2utf8(m_AppId).data(), gbk2utf8(m_AppId).size(), gbk2utf8(m_Account).data(), gbk2utf8(m_Account).size(), gbk2utf8(m_channelKey).data(), gbk2utf8(m_channelKey).size(), 0, "", 0, retryTime, retryCount);
```

```
// 设置登录成功回调
void CSingleCallBack::onLoginSuccess(uint32_t uid, int fd){
      // Your code
}
```

```
// 设置登录失败回调
void CSingleCallBack::onLoginFailed(int ecode){
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
void CSingleCallBack::onMessageChannelReceive(char const * channelID, size_t channelID_size, char const * account, size_t account_size, uint32_t uid, char const * msg, size_t msg_size){
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
m_AgoraAPI->logout();
```


