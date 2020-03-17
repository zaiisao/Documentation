
---
title: 客户端：发送点对点文本消息和频道文本消息
description: 
platform: Android
updatedAt: Mon Nov 18 2019 06:42:09 GMT+0800 (CST)
---
# 客户端：发送点对点文本消息和频道文本消息
## 第一节：快速集成

本章介绍信令 SDK 的集成方法。

### 第一步：准备开发环境。

请确保开发环境至少满足以下需求：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>平台</td>
<td>系统要求</td>
<td>网络要求</td>
</tr>
<tr><td>Android</td>
<td>Android API 15或以上</td>
<td>HTTP 80 端口，TCP 8181 端口</td>
</tr>
</tbody>
</table>


### 第二步：下载最新的 Agora Signaling SDK 软件包。

下载 [Agora Signaling SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads) 并将其中 **libs** 文件夹内的库复制到你的项目文件夹里。

### 第三步：

解压已下载的 SDK 软件包。

### 第四步：复制解压包中的 libs 文件夹。

1.  将其中的 **libs** 文件夹下的 **.jar** 文件 复制到本项目的 **app/libs** 下。

2.  将其中的 **arm64-v8a/x86/armeabi-v7a** 复制到本项目的 **app/src/main/libs** 下。


### 第五步：设置 Android 属性

在您项目的 `app/build.gradle` 文件的 android 属性中添加如下代码：

```
sourceSets {
     main {
     jniLibs.srcDirs = ['src/main/libs']
          }
       }
```

### 第六步：添加依赖关系

在您项目的 `app/build.gradle` 文件的文件依赖属性中添加如下依赖关系：

```
compile fileTree(include: ['*.jar'], dir: 'libs')
```

### 第七步：同步项目

点击 **Sync Project with Gradle Files** 同步项目。

### 第八步：获取 App ID 和 App 证书。

详见 [SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

### 第九步：计算 token

建议您在服务器端计算 token 动态秘钥。token 计算方法详见：[SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

您现在已经准备就绪，可以调用 Agora Signaling SDK 提供的 API 了。

## 第二节：信令相关场景介绍及相关代码

本节提供如下两种场景的代码片段：

-   发送点对点消息

-   发送频道消息


### 发送点对点消息

本章提供发送点对点消息相关的示例代码。

#### 发送端

```
// 初始化信令 SDK
m_agoraAPI = AgoraAPIOnlySignal.getInstance(context, appID);
```

```
// 登录 Agora 信令系统
m_agoraAPI.login2(appId, account, token, uid, deviceID, retry_time_in_s, retry_count)
```

```
// 设置登录成功回调
m_agoraAPI.onLoginSuccess(uid, fd) {
      // Your code
}
```

```
// 设置登录失败回调
m_agoraAPI.onLoginFailed(ecode) {
      // Your code
}
```

```
// 发送点对点消息
m_agoraAPI.messageInstantSend(account, uid, msg, msgID)
```

```
// 设置消息发送成功回调
m_agoraAPI.onMessageSendSuccess(messageID){
     // Your code
}
```

```
// 设置消息发送失败回调
m_agoraAPI.onMessageSendError(messageID, ecode) {
//Your code
}
```

```
// 退出 Agora 信令系统
m_agoraAPI.logout()
```

#### 接收端（对端）

```
// 初始化信令 SDK
m_agoraAPI = AgoraAPIOnlySignal.getInstance(context, appID);
```

```
// 登录 Agora 信令系统
m_agoraAPI.login2(appId, account, token, uid, deviceID, retry_time_in_s, retry_count)
```

```
// 设置登录成功回调
m_agoraAPI.onLoginSuccess(uid, fd) {
      // Your code
}
```

```
// 设置登录失败回调
m_agoraAPI.onLoginFailed(ecode) {
      // Your code
}
```

```
 // 设置对端收到消息回调
 m_agoraAPI.onMessageInstantReceive(account, uid, msg){
      // Your code
}
```

```
// 退出 Agora 信令系统
m_agoraAPI.logout()
```

### 发送频道消息

本章提供发送频道消息相关的示例代码。

#### 发送端

```
// 初始化信令 SDK
m_agoraAPI = AgoraAPIOnlySignal.getInstance(context, appID);
```

```
// 登录 Agora 信令系统
m_agoraAPI.login2(appId, account, token, uid, deviceID, retry_time_in_s, retry_count)
```

```
// 设置登录成功回调
m_agoraAPI.onLoginSuccess(uid,fd) {
      // Your code
}
```

```
// 设置登录失败回调
m_agoraAPI.onLoginFailed(ecode){
      // Your code
}
```

```
// 加入频道
m_agoraAPI.channelJoin(channelName)
```

```
// 设置加入频道成功回调
m_agoraAPI.onChannelJoined(channelID){
     // Your code
}
```

```
// 设置加入频道失败回调
m_agoraAPI.onChannelJoinFailed(channelID, ecode) {
      // Your code
}
```

```
// 发送频道消息
m_agoraAPI.messageChannelSend(channelName, msg, msgID)
```

```
// 设置消息发送成功回调
m_agoraAPI.onMessageSendSuccess(messageID) {
      // Your code
}
```

```
// 设置消息发送失败回调
m_agoraAPI.onMessageSendError(messageID, ecode){
      // Your code
}
```

```
// 退出频道
m_agoraAPI.channelLeave(channelName)
```

```
// 设置退出频道回调
m_agoraAPI.onChannelLeaved(channelID， ecode) {
      // Your code
}
```

```
// 退出 Agora 信令系统
m_agoraAPI.logout()
```

#### 接收端 （对端）

```
// 初始化信令 SDK
m_agoraAPI = AgoraAPIOnlySignal.getInstance(context, appID);
```

```
// 登录 Agora 信令系统
m_agoraAPI.login2(appId, account, token, uid, deviceID, retry_time_in_s, retry_count)
```

```
// 设置登录成功回调
m_agoraAPI.onLoginSuccess(uid,fd) {
      // Your code
}
```

```
// 设置登录失败回调
m_agoraAPI.onLoginFailed(ecode) {
      // Your code
}
```

```
// 加入频道
m_agoraAPI.channelJoin(channelName)
```

```
// 设置加入频道成功回调
m_agoraAPI.onChannelJoined(channelID) {
     // Your code
}
```

```
// 设置加入频道失败回调
m_agoraAPI.onChannelJoinFailed(channelID, ecode) {
      // Your code
}
```

```
// 设置对端接收到频道消息回调
m_agoraAPI.onMessageChannelReceive(channelID, account, uid, msg) {
      // Your code
}
```

```
// 退出频道
m_agoraAPI.channelLeave(channelName)
```

```
// 设置退出频道回调
m_agoraAPI.onChannelLeaved(channelID， ecode) {
      // Your code
}
```

```
// 退出 Agora 信令系统
m_agoraAPI.logout()
```


