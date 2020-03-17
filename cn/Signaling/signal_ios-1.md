
---
title: 客户端：发送点对点文本消息和频道文本消息
description: 
platform: iOS
updatedAt: Mon Nov 18 2019 06:42:11 GMT+0800 (CST)
---
# 客户端：发送点对点文本消息和频道文本消息
## 第一节：快速集成

### 第一步：准备开发环境。

请确保开发环境至少满足以下需求：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>平台</strong></td>
<td><strong>系统要求</strong></td>
<td><strong>网络要求</strong></td>
</tr>
<tr><td>iOS</td>
<td>iOS 7.0 或以上版本</td>
<td>HTTP 80 端口，TCP 8181 端口</td>
</tr>
</tbody>
</table>




> 真机或模拟器均可。

### 第二步：下载最新的 Agora Signaling SDK 软件包。

下载 [Agora Signaling SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads)。

### 第三步：解压已下载的 SDK 软件包的 libs 并将其复制到对应的项目文件夹中。

> Agora Signaling SDK 默认使用 libc++ (LLVM) 。 SDK 提供的库是 FAT Image，包含 32/64 位模拟器、32/64 位真机版本。

### 第四步：添加所需 Framework

您可以手动添加所需库：

1.  选择当前的 **Target** 。

2.  在 Xcode 里设置 framework 的 search path 为 **Agora SDK lib** 文件夹。

3.  点击 **Building Phases** 添加所需库。

     <img alt="../_images/xcode_4.jpeg" src="https://web-cdn.agora.io/docs-files/cn/xcode_4.jpeg" style="width: 1122.0px; height: 374.0px;"/>

4.  点开 **Link Binary with Libraries** 并点击 **+** 添加下列库。

    ![](https://web-cdn.agora.io/docs-files/1537867403872)
		
> `AgoraSigKit.framework` 位于您项目文件夹的 **libs** 文件夹内。因此，点击 **+** 后您还需点击 **Add Other…** 进入项目文件夹添加。
> 
> <img alt="../_images/xcode_5.jpeg" src="https://web-cdn.agora.io/docs-files/cn/xcode_5.jpeg" style="width: 491.0px; height: 196.0px;"/>


### 第五步：其他设置

1.  选择当前的 **Target** 。

2.  根据实际需要，选择启用或禁用 **Bitcode** ：

    <img alt="../_images/bitcode.jpeg" src="https://web-cdn.agora.io/docs-files/cn/bitcode.jpeg" style="width: 749.0px; height: 191.0px;"/>



### 第七步：获取 App ID 和 App 证书。

详见 [SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

### 第八步：计算 token

建议您在服务器端计算 token 动态秘钥。token 计算方法详见：[SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

您现在已经准备就绪，可以调用 Agora Signaling SDK 提供的 API 了。

## 第二节：信令相关场景介绍及相关代码

本节提供如下两种场景的代码片段：

-   发送点对点消息

-   发送频道消息


### 发送点对点消息

#### 发送端

```
// 初始化信令 SDK
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
// 登录 Agora 信令系统
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
// 设置登录成功回调
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      // Your code
}
```

```
// 设置登录失败回调
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      // Your code
}
```

```
// 发送点对点消息
AgoraSignalKit.messageInstantSend(account, uid: 0, msg: message, msgID: msgID)
```

```
// 设置消息发送成功回调
AgoraSignalKit.onMessageSendSuccess = { (messageID) -> () in
     // Your code
}
```

```
// 设置消息发送失败回调
AgoraSignalKit.onMessageSendError = { (messageID, ecode) -> () in
//Your code
}
```

```
// 退出 Agora 信令系统
AgoraSignalKit.logout()
```

#### 接收端（对端）

```
// 初始化信令 SDK
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
// 登录 Agora 信令系统
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
// 设置登录成功回调
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      // Your code
}
```

```
// 设置登录失败回调
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      // Your code
}
```

```
 // 设置对端收到消息回调
 AgoraSignalKit.onMessageInstantReceive = { (account, uid, msg) -> () in
      // Your code
}
```

```
// 退出 Agora 信令系统
AgoraSignalKit.logout()
```

### 发送频道消息

#### 发送端

```
// 初始化信令 SDK
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
// 登录 Agora 信令系统
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
// 设置登录成功回调
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      // Your code
}
```

```
// 设置登录失败回调
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      // Your code
}
```

```
// 加入频道
AgoraSignalKit.channelJoin(channelName)
```

```
// 设置加入频道成功回调
AgoraSignalKit.onChannelJoined = { (channelID) -> () in
     // Your code
}
```

```
// 设置加入频道失败回调
AgoraSignalKit.onChannelJoinFailed = { (channelID, ecode) -> () in
      // Your code
}
```

```
// 发送频道消息
AgoraSignalKit.messageChannelSend(channelName, msg: message, msgID: msgID)
```

```
// 设置消息发送成功回调
AgoraSignalKit.onMessageSendSuccess = { (messageID) -> () in
      // Your code
}
```

```
// 设置消息发送失败回调
AgoraSignalKit.onMessageSendError = { (messageID, ecode) -> () in
      // Your code
}
```

```
// 退出频道
AgoraSignalKit.channelLeave(channelName)
```

```
// 设置退出频道回调
AgoraSignalKit.onChannelLeaved = { (channelID， ecode) -> () in
      // Your code
}
```

```
// 退出 Agora 信令系统
AgoraSignalKit.logout()
```

#### 接收端 （对端）

```
// 初始化信令 SDK
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
// 登录 Agora 信令系统
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
// 设置登录成功回调
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      // Your code
}
```

```
// 设置登录失败回调
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      // Your code
}
```

```
// 加入频道
AgoraSignalKit.channelJoin(channelName)
```

```
// 设置加入频道成功回调
AgoraSignalKit.onChannelJoined = { (channelID) -> () in
     // Your code
}
```

```
// 设置加入频道失败回调
AgoraSignalKit.onChannelJoinFailed = { (channelID, ecode) -> () in
      // Your code
}
```

```
// 设置对端接收到频道消息回调
AgoraSignalKit.onMessageChannelReceive = { (channelID, account, uid, msg) -> () in
      // Your code
}
```

```
// 退出频道
AgoraSignalKit.channelLeave(channelName)
```

```
// 设置退出频道回调
AgoraSignalKit.onChannelLeaved = { (channelID， ecode) -> () in
      // Your code
}
```

```
// 退出 Agora 信令系统
AgoraSignalKit.logout()
```


