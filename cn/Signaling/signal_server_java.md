
---
title: 在 Java 平台服务器上发送点对点文本消息和频道文本消息
description: 
platform: Java
updatedAt: Mon Nov 18 2019 06:42:13 GMT+0800 (CST)
---
# 在 Java 平台服务器上发送点对点文本消息和频道文本消息
## 第一节：快速集成

本章介绍信令 SDK 的集成方法。

### 第一步：准备开发环境。

请确保开发环境至少满足以下需求：

-   Java 1.5 及以上

-   IDE 必须支持 Gradle


### 第二步：下载最新的 Agora Signaling SDK 软件包。

下载 [Agora Signaling SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads)。

### 第三步：

解压已下载的 SDK 软件包。

### 第四步：复制解压包中的 libs 文件夹。

将其中的 **libs** 文件夹下和 **libs-dep** 下所有 的 **.jar** 文件复制到本项目的 **libs** 下。

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
Signal signal = new Signal(appId);
```

```
// 登录 Agora 信令系统
signal.login(accountName, this.token, new Signal.LoginCallback()）
```

```
// 设置登录成功回调
public void onLoginSuccess(final Signal.LoginSession session, int uid)  {
      // Your code
}
```

```
// 设置登录失败回调
public void onLoginFailed(LoginSession session, int ecode)  {
      // Your code
}
```

```
// 发送点对点消息
Signal.LoginSession currentSession = users.get(currentUser).getSession();
currentSession.messageInstantSend(oppositeAccount, msg, new Signal.MessageCallback()
```

```
// 设置消息发送成功回调
public void onMessageSendSuccess(Signal.LoginSession session) {
     // Your code
}
```

```
// 设置消息发送失败回调
public void onMessageSendError(Signal.LoginSession session, int ecode)  {
//Your code
}
```

```
// 退出 Agora 信令系统
currentSession.logout()
```

#### 接收端（对端）

```
// 初始化信令 SDK
Signal signal = new Signal(appId);
```

```
// 登录 Agora 信令系统
signal.login(accountName, this.token, new Signal.LoginCallback()）
```

```
// 设置登录成功回调
public void onLoginSuccess(final Signal.LoginSession session, int uid) {
      // Your code
}
```

```
// 设置登录失败回调
public void onLoginFailed(LoginSession session, int ecode)  {
      // Your code
}
```

```
 // 设置对端收到消息回调
 public void onMessageInstantReceive(Signal.LoginSession session, String account, int uid, String msg) {
      // Your code
}
```

```
// 退出 Agora 信令系统
currentSession.logout()
```

### 发送频道消息

以下为发送频道消息相关的示例代码。

#### 发送端

```
// 初始化信令 SDK
Signal signal = new Signal(appId);
```

```
// 登录 Agora 信令系统
signal.login(accountName, this.token, new Signal.LoginCallback()）
```

```
// 设置登录成功回调
public void onLoginSuccess(final Signal.LoginSession session, int uid)  {
      // Your code
}
```

```
// 设置登录失败回调
public void onLoginFailed(LoginSession session, int ecode) {
      // Your code
}
```

```
// 加入频道
final CountDownLatch channelJoindLatch = new CountDownLatch(1);
Channel channel = users.get(currentUser).getSession().channelJoin(channelName, new Signal.ChannelCallback()
```

```
// 设置加入频道成功回调
public void onChannelJoined(Signal.LoginSession session, Signal.LoginSession.Channel channelName) {
     // Your code
}
```

```
// 设置加入频道失败回调
public void onChannelJoinFailed(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode) {
      // Your code
}
```

```
// 发送频道消息
users.get(currentUser).getChannel().messageChannelSend(msg)
```

```
// 设置消息发送成功回调
public void onMessageSendSuccess(Signal.LoginSession session) {
     // Your code
}
```

```
// 设置消息发送失败回调
public void onMessageSendError(Signal.LoginSession session, int ecode)  {
//Your code
}
```

```
// 退出频道
users.get(currentUser).getChannel().channelLeave()
```

```
// 设置退出频道回调
public void onChannelLeaved(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode) {
      // Your code
}
```

```
// 退出 Agora 信令系统
currentSession.logout()
```

#### 接收端 （对端）

```
// 初始化信令 SDK
Signal signal = new Signal(appId);
```

```
// 登录 Agora 信令系统
signal.login(accountName, this.token, new Signal.LoginCallback()
```

```
// 设置登录成功回调
public void onLoginSuccess(final Signal.LoginSession session, int uid) {
      // Your code
}
```

```
// 设置登录失败回调
public void onLoginFailed(LoginSession session, int ecode)  {
      // Your code
}
```

```
// 加入频道
final CountDownLatch channelJoindLatch = new CountDownLatch(1);
Channel channel = users.get(currentUser).getSession().channelJoin(channelName, new Signal.ChannelCallback()
```

```
// 设置加入频道成功回调
public void onChannelJoined(Signal.LoginSession session, Signal.LoginSession.Channel channelName)  {
     // Your code
}
```

```
// 设置加入频道失败回调
public void onChannelJoinFailed(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode) {
      // Your code
}
```

```
// 设置对端接收到频道消息回调
public void onMessageChannelReceive(Signal.LoginSession session, Signal.LoginSession.Channel channel, String account, int uid, String msg) {
      // Your code
}
```

```
// 退出频道
users.get(currentUser).getChannel().channelLeave()
```

```
// 设置退出频道回调
public void onChannelLeaved(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode) {
      // Your code
}
```

```
// 退出 Agora 信令系统
currentSession.logout()
```


