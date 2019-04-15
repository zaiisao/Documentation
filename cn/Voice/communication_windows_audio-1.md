
---
title: 实现语音通话
description: 
platform: Windows
updatedAt: Thu Nov 01 2018 08:19:56 GMT+0800 (CST)
---
# 实现语音通话
# 实现语音通话

本文介绍如何使用 Agora SDK 在 Windows 平台上实现语音通话。

## 准备环境

-   具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/windows_video.md) 。

-   参考 [示例项目](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-Windows) 了解如何从头创建一个示例项目。


## 快速开始

### 创建 AgoraRtcEngine 实例并初始化

进入语音通话频道之前，调用 <code>create</code> 方法创建一个 <code>AgoraRtcEngine</code> 实例，并调用 <code>initialize</code> 方法完成初始化。

填入获取到的 App ID 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

### 加入频道

现在你可以加入语音通话频道了。 调用 <code>joinChannel</code> 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。

-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。


> 如果已在通话中，则必须调用 <code>leaveChannel</code> 方法退出当前通话，才能进入下一个频道。

```
nRet = m_lpAgoraEngine->joinChannel(NULL, lpChannelName, NULL, nUID);
```

### 结束语音通话

语音通话结束时，调用 <code>leaveChannel</code> 方法离开频道。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。真正退出频道后，SDK 会触发 <code>onleaveChannel</code> 回调。

> 如果在调用 <code>leaveChannel</code> 方法后立即使用 <code>release</code>，则退出频道会被打断，SDK 也不会触发 <code>onleaveChannel</code> 回调。

```
int nRet = m_lpAgoraEngine->leaveChannel();
```

## 结论

现在你可以开始一个一对一或一对多语音通话了。

## 参考

在上述语音通话过程中，我们使用了 Agora SDK 提供的部分 API 功能：

-   创建 <code>agora::IRtcEngine</code> 对象：<code>create</code>

-   初始化对象：<code>initialize</code>

-   加入频道：<code>joinChannel</code>

-   离开频道：<code>leaveChannel</code>

-   销毁 <code>IRtcEngine</code> 对象：<code>release</code>


更多功能实现，请参考 [语音通话 API](https://docs.agora.io/cn/Voice/API%20Reference/cpp/index.html) 中各 API 的功能及描述。



