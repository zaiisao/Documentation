
---
title: 实现语音直播
description: 
platform: Linux
updatedAt: Fri Sep 28 2018 19:41:45 GMT+0800 (CST)
---
# 实现语音直播
# 实现语音直播

参考本文，了解如何使用 Agora SDK 在 Linux 平台上实现语音直播。

## 准备环境

-   具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/linux_audio.md) 。

-   参考 [https://github.com/AgoraIO/OpenVideoCall-Linux/tree/dev/2.3.0](https://github.com/AgoraIO/OpenVideoCall-Linux/tree/dev/2.3.0) 了解如何从头创建一个示例项目。


## 快速开始

### 初始化 AgoraRtcEngine

进入语音直播频道之前，调用 <code>create</code> 方法创建一个 AgoraRtcEngine 实例，并调用 <code>initialize</code> 方法完成初始化。

填入获取到的 App ID 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。

```
AGEngine::AGEngine(IRtcEngineEventHandler* handler, const char* appId)
{
    m_agoraEngine = createAgoraRtcEngine();
    RtcEngineContext ctx;
    ctx.eventHandler = handler;
    ctx.appId = appId;
    m_agoraEngine->initialize(ctx);

    m_parameters = new RtcEngineParameters(m_agoraEngine);
}
```

### 设置频道模式

初始化实例后，调用 <code>setChannelProfile</code> 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。

在该方法中，将频道模式设置为直播模式。直播模式有主播和观众两种用户角色。主播可收发语音，但观众只能收，不能发。

> -   该方法必须在加入频道前调用才能生效。

> -   一个 Engine 只能设置一种频道模式。如果需要改变频道模式，请先调用 <code>release</code> 销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。


```
ret = m_agoraEngine->setChannelProfile(CHANNEL_PROFILE_LIVE_BROADCASTING);
```

### 设置用户角色

直播频道分主播和观众两种用户角色。在将频道模式为直播后，调用 <code>setClientRole</code> 方法，并根据需要将用户设置为主播或观众。两者的区别在于：

-   主播：可以收听和发布音视频消息。根据应用程序的实现，还可以与观众互动、指定观众连麦。同一直播频道内，主播只能听到和看到自己以及连麦主播的音视频。

-   观众：只能收听主播的音视频消息。根据应用程序的实现，还可以发布实时文字消息，与主播互动。同一直播频道内，所有观众都能听到和看到主播以及连麦主播的音视频。


> 在加入直播频道前，务必调用该方法设置用户角色。直播过程中，用户也可以再次调用该方法，改变设置的用户角色。

```
m_agoraEngine->setClientRole(CLIENT_ROLE_BROADCASTER); //设置为主播
m_agoraEngine->setClientRole(CLIENT_ROLE_AUDIENCE); //设置为观众
```

### 加入频道

现在你可以加入语音直播频道了。 调用 <code>joinChannel</code> 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。

-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。


> 如果已在通话中，则必须调用 <code>leaveChannel</code> 方法退出当前通话，才能进入下一个频道。

```
ret = m_agoraEngine->joinChannel(NULL, channelId, NULL, uid);
```

### 结束语音直播

语音直播结束时，调用 <code>leaveChannel</code> 方法离开频道。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。真正退出频道后，SDK 会触发 <code>onleaveChannel</code> 回调。

> 如果在调用 <code>leaveChannel</code> 方法后立即使用 <code>release</code>，则退出频道会被打断，SDK 也不会触发 <code>onleaveChannel</code> 回调。

```
int ret = m_agoraEngine->leaveChannel();
```

## 结论

现在你可以开始互动直播了。

## 参考

在上述语音直播过程中，我们使用了 Agora SDK 提供的部分 API 功能：

-   创建 agora::IRtcEngine 对象：<code>create</code>

-   初始化对象：<code>initialize</code>

-   设置频道属性：<code>setChannelProfile</code>

-   设置用户角色：<code>setClientRole</code>

-   打开视频模式：<code>enableVideo</code>

-   设置本地视频属性：<code>setVideoProfile</code>

-   销毁 IRtcEngine 对象：<code>release</code>

-   加入频道：<code>joinChannel</code>

-   离开频道：<code>leaveChannel</code>


更多功能实现，请参考 [互动直播 API](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/index.html) 中各 API 的功能及描述。


