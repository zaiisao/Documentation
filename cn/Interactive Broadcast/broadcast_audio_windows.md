
---
title: 实现语音直播
description: 
platform: Windows
updatedAt: Wed Oct 17 2018 06:53:09 GMT+0800 (CST)
---
# 实现语音直播
# 实现语音直播

参考本文，了解如何使用 Agora SDK 在 Windows 平台上实现语音直播。

## 准备环境

-   具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/windows_video.md) 。

-   参考 [示例项目](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-Windows) 了解如何从头创建一个示例项目。


## 快速开始

### 创建 AgoraRtcEngine 实例并初始化

进入语音直播频道之前，调用 <code>create</code> 方法创建一个 AgoraRtcEngine 实例，并调用 <code>initialize</code> 方法完成初始化。

填入获取到的 App ID 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

### 设置频道模式

初始化实例后，调用 <code>setChannelProfile</code> 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。

在该方法中，将频道模式设置为直播模式。直播模式适用于互动直播场景。频道内有主播和观众两种角色，主播收发音频消息，观众只收不发。

> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。


```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_LIVE_BROADCASTING);
```

### 设置用户角色

直播频道分主播和观众两种用户角色。在将频道模式为直播后，调用 <code>setClientRole</code> 方法，并根据需要将用户设置为主播或观众。两者的区别在于：

-   主播：可以收听和发布音频消息。根据应用程序的实现，还可以与观众互动、指定观众连麦。同一直播频道内，主播只能听到和看到自己以及连麦主播的音频。
-   观众：只能收听主播的音频消息。根据应用程序的实现，还可以发布实时文字消息，与主播互动。同一直播频道内，所有观众都能听到和看到主播以及连麦主播的音频。
> 在加入直播频道前，务必调用该方法设置用户角色。直播过程中，用户也可以再次调用该方法，改变设置的用户角色。

```
int nRet = m_lpAgoraEngine->setClientRole(role);
```

### 加入频道

现在你可以加入语音直播频道了。 调用 <code>joinChannel</code> 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。

-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。


> 如果已在通话中，则必须调用 <code>leaveChannel</code> 方法退出当前通话，才能进入下一个频道。

```
nRet = m_lpAgoraEngine->joinChannel(NULL, lpChannelName, NULL, nUID);
```

### 结束语音直播

语音直播结束时，调用 <code>leaveChannel</code> 方法离开频道。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。真正退出频道后，SDK 会触发 <code>onleaveChannel</code> 回调。

> 如果在调用 <code>leaveChannel</code> 方法后立即使用 <code>release</code>，则退出频道会被打断，SDK 也不会触发 <code>onleaveChannel</code> 回调。

```
int nRet = m_lpAgoraEngine->leaveChannel();
```

## 结论

现在你可以开始语音直播了。

## 参考

在上述语音直播过程中，我们使用了 Agora SDK 提供的部分 API 功能：

-   创建 agora::IRtcEngine 对象： <code>create</code>

-   初始化： <code>initialize</code>

-   设置频道属性：<code>setChannelProfile</code>

-   销毁IRtcEngine对象： <code>release</code>

-   设置用户角色： <code>setClientRole</code>

-   加入频道： <code>joinChannel</code>

-   离开频道： <code>leaveChannel</code>


更多功能实现，请参考 [互动直播 API](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/index.html) 中各 API 的功能及描述。



