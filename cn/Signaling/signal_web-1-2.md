
---
title: 客户端：发送点对点文本消息和频道文本消息
description: 
platform: Web
updatedAt: Mon Nov 18 2019 06:42:12 GMT+0800 (CST)
---
# 客户端：发送点对点文本消息和频道文本消息
## 第一节：快速集成

### 第一步：下载最新的 Agora Signaling SDK 软件包。

下载地址： [Agora Signaling SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads)。

### 第二步：解压已下载的 SDK 软件包。

将解压后的 **SDK** 包全部复制到您的项目文件夹 **web/src/assets/vendor/** 下。

### 第三步：获取 App ID 和 App 证书。

详见 [SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

### 第四步：计算 token

建议您在服务器端计算 token 动态秘钥。token 计算方法详见：[SignalingToken](../../cn/Agora%20Platform/key_signaling.md)。

您现在已经准备就绪，可以调用 Agora Signaling SDK 提供的 API 了。

## 第二节：信令相关场景介绍及相关代码

本节提供如下两种场景的代码片段：

-   发送点对点消息

-   发送频道消息


### 发送点对点消息

```
 //登录
 var session = signal.login(account, token);
 session.onLoginSuccess = function(uid){
//加入频道
session.onMessageInstantReceive = function(account, uid, msg){
//接收点对点消息回调设置
};
//发送点对点消息
session.messageInstantSend(receiver, msg);

//登出
session.logout();
};

session.onLogout = function(ecode){
//登出回调设置
}
```

### 发送频道消息

```
//登录
var session = signal.login(account, token);
session.onLoginSuccess = function(uid){
//加入频道，设置加入频道成功和失败回调
var channel = session.channelJoin(channelname);
channel.onChannelJoined = function(){
channel.onMessageChannelReceive = function(account, uid, msg){
//接收频道消息回调设置
}
//发送频道消息
channel.messageChannelSend(text);

//登出
session.logout();
 };
};

session.onLogout = function(ecode){
//登出回调设置
}
```

> 如需使用小程序，请在微信白名单添加以下域名：
> - https://lbs-wx.agora.io
> - ap1-wx.agoraio.cn
> - wss://ap1-wx.agora.io 

