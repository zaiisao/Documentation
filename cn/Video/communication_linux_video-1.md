
---
title: 实现视频通话
description: 
platform: Linux
updatedAt: Fri Sep 28 2018 18:16:21 GMT+0800 (CST)
---
# 实现视频通话
# 实现视频通话

参考本文，了解如何使用 Agora SDK 在 Linux 平台上实现视频通话。

> 在 Linux 平台上实现视频通话时，如果你希望看到视频图像，可以获得视频裸数据，并自行渲染。关于视频裸数据的接口，详见 <code>onCaptureVideoFrame</code> 和 <code>onRenderVideoFrame</code> 。

## 准备环境

-   具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/linux_audio.md) 。

-   参考 [https://github.com/AgoraIO/OpenVideoCall-Linux/tree/dev/2.3.0](https://github.com/AgoraIO/OpenVideoCall-Linux/tree/dev/2.3.0) 了解如何从头创建一个示例项目。


## 快速开始

### 初始化 AgoraRtcEngine

进入视频通话频道之前，调用 <code>create</code> 方法创建一个 AgoraRtcEngine 实例，并调用 <code>initialize</code>  方法完成初始化。

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

初始化实例后，调用 <code>setChannelProfile</code>  方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。

在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。

> -   该方法必须在加入频道前调用才能生效。

> -   一个 Engine 只能设置一种频道模式。如果需要改变频道模式，请先调用 <code>release</code>  销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。


```
ret = m_agoraEngine->setChannelProfile(CHANNEL_PROFILE_COMMUNICATION);
```

### 打开视频模式

调用 <code>enableVideo</code>  方法打开视频模式。在加入频道前，或通话过程中，你都可以调用该方法开启视频。音频功能是默认打开的。

-   如果在加入频道前打开视频模式，则进入频道后直接加入视频通话。

-   如果在通话过程中打开视频模式，则由纯音频通话切换为视频通话。


```
ret = m_agoraEngine->enableVideo();
```

### 设置视频编码属性

打开视频模式后，调用 <code>setVideoProfile</code> 方法设置视频编码属性。

在该方法中：

-   将 <code> AgoraVideoProfile</code>  设置为你想要的值，如 <code> VIDEO\_PROFILE\_360P</code>  。该参数对应着一套视频相关的属性，包含视频分辨率、帧率、码率等。

-   将 <code> swapWidthAndHeight</code> 根据实际需要设置为 true  或者  false。如果设置为  true，则交换视频的宽和高，反之则不交换。


>   Agora 建议直接通过 <code>AgoraVideoProfile</code> 来定义输出视频的 Orientation 模式；<code>swapWidthAndHeight</code> 建议设为默认的 false。

-   如果设备的摄像头无法支持定义的视频属性，SDK 会为摄像头自动选择一个合理的分辨率。该行为对视频编码没有影响，编码时 SDK 仍沿用该方法中定义的分辨率。


```
bool AGEngine::setVideoProfile(int videoProfile)
{
    int ret = m_agoraEngine->setVideoProfile((VIDEO_PROFILE_TYPE)videoProfile, false);

    return ret == 0 ? true : false;
}
```

### 加入频道

现在你可以加入视频通话频道了。 调用 <code>joinChannel</code>  方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。

-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。


> 如果已在通话中，则必须调用 <code>leaveChannel</code>  方法退出当前通话，才能进入下一个频道。

```
ret = m_agoraEngine->joinChannel(NULL, channelId, NULL, uid);
```

### 结束视频通话

视频通话结束时，调用 <code>leaveChannel</code>  方法离开频道。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。真正退出频道后，SDK 会触发 <code>onleaveChannel</code>  回调。

> 如果在调用 <code>leaveChannel</code>  方法后立即使用 <code>release</code> ，则退出频道会被打断，SDK 也不会触发 <code>onleaveChannel</code> 回调。

```
int ret = m_agoraEngine->leaveChannel();
```

## 结论

现在你可以开始一个一对一或一对多视频通话了。

## 参考

在上述视频通话过程中，我们使用了 Agora SDK 提供的部分 API 功能：

-   创建 agora::IRtcEngine 对象： <code>create</code> 

-   初始化 IRtcEngine 对象：<code>initialize</code> 

-   设置频道属性：<code>setChannelProfile</code> 

-   打开视频模式：<code>enableVideo</code> 

-   设置本地视频属性：<code>setVideoProfile</code> 

-   销毁 IRtcEngine 对象：<code>release</code> 

-   加入频道：<code>joinChannel</code> 

-   离开频道：<code>leaveChannel</code> 

-   获得其他用户的视频：<code>onRenderVideoFrame</code> 


更多功能实现，请参考 [视频通话 API](https://docs.agora.io/cn/Video/API%20Reference/cpp/index.html)中各 API 的功能及描述。



