
---
title: 实现语音通话
description: 
platform: Linux
updatedAt: Fri Sep 28 2018 18:16:12 GMT+0800 (CST)
---
# 实现语音通话
# 实现语音通话

参考本文，了解如何使用 Agora SDK 在 Linux 平台上实现语音通话。

## 准备环境

-   具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/linux_audio.md) 。

-   参考 [https://github.com/AgoraIO/OpenVideoCall-Linux/tree/dev/2.3.0](https://github.com/AgoraIO/OpenVideoCall-Linux/tree/dev/2.3.0) 了解如何从头创建一个示例项目。


## 快速开始

### 初始化 AgoraRtcEngine

进入语音通话频道之前，调用 <code>create</code> 方法创建一个 <code>AgoraRtcEngine</code> 实例，并调用 <code>initialize</code> 方法完成初始化。完成初始化后，默认处于语音通道模式。

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

在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。

> -   该方法必须在加入频道前调用才能生效。
> -   一个 Engine 只能设置一种频道模式。如果需要改变频道模式，请先调用 <code>release</code> 销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。



```
ret = m_agoraEngine->setChannelProfile(CHANNEL_PROFILE_COMMUNICATION);
```

### 加入频道

现在你可以加入语音通话频道了。 调用 <code>joinChannel</code> 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。

-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。


> 如果已在通话中，则必须调用 <code>leaveChannel</code> 方法退出当前通话，才能进入下一个频道。



```
ret = m_agoraEngine->joinChannel(NULL, channelId, NULL, uid);
```

### 结束语音通话

语音通话结束时，调用 <code>leaveChannel</code> 方法离开频道。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。真正退出频道后，SDK 会触发 <code>onleaveChannel</code> 回调。

> 如果在调用 <code>leaveChannel</code> 方法后立即使用 <code>release</code>，则退出频道会被打断，SDK 也不会触发 <code>onleaveChannel</code> 回调。



```
int ret = m_agoraEngine->leaveChannel();
```

## 结论

现在你可以开始一个一对一通话或多人群聊了。

## 参考

在上述语音通话过程中，我们使用了 Agora SDK 提供的部分 API 功能：

-   创建 agora::IRtcEngine 对象： <code>create</code>

-   初始化对象：<code>initialize</code>

-   设置频道属性： <code>setChannelProfile</code>

-   销毁 IRtcEngine 对象： <code>release</code>

-   加入频道：<code>joinChannel</code>

-   离开频道：<code>leaveChannel</code>


更多功能实现，请参考 [语音通话 API](https://docs.agora.io/cn/Voice/API%20Reference/cpp/index.html)  中各 API 的功能及描述。



