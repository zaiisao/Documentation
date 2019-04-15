
---
title: 实现视频通话
description: 
platform: Windows
updatedAt: Thu Nov 01 2018 08:15:28 GMT+0800 (CST)
---
# 实现视频通话
# 实现视频通话

参考本文，了解如何使用 Agora SDK 在 Windows 平台上实现视频通话。

## 准备环境

-   具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/windows_video.md) 。

-   参考 [示例项目](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-Windows) 了解如何从头创建一个示例项目。


## 快速开始

### 创建 AgoraRtcEngine 实例并初始化

进入视频通话频道之前，调用 <code>create</code> 方法创建一个 AgoraRtcEngine 实例，并调用 <code>initialize</code> 方法完成初始化。

填入获取到的 App ID 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

### 打开视频模式

调用 <code>enableVideo</code> 方法打开视频模式。在加入频道前，或通话过程中，你都可以调用该方法开启视频。音频功能是默认打开的。

-   如果在加入频道前打开视频模式，则进入频道后直接加入视频通话。

-   如果在通话过程中打开视频模式，则由纯音频通话切换为视频通话。


```
nRet = m_lpAgoraEngine->enableVideo();
```

### 设置本地视频视图

本地视频视图，是指用户在本地设备上看到的本地视频流的视图。

在进入频道前调用 <code>setupLocalVideo</code> 方法，使应用程序绑定本地视频流的显示视窗，并设置本地看到的本地视频视图。

在该方法中：

-   选择你想要看到的本地视频流的视窗。

-   指定你想要的视频显示模式。共有两种模式可选：Hidden 和 Fit 模式。两种模式下视频尺寸都是等比缩放，区别在于：

    -   Hidden 模式：优先保证视窗被填满。如果缩放后的视频尺寸与显示视窗尺寸不一致，多出的视频将被截掉。

    -   Fit 模式：优先保证视频内容全部显示。如果缩放后的视频尺寸与显示视窗尺寸不一致，未被填满的视窗区域将使用黑色填充。

-   传入本地用户的 UID。该值默认为 0，可以不设置。


```
VideoCanvas vc;

vc.uid = 0;
vc.view = m_wndLocal.GetSafeHwnd();
vc.renderMode = RENDER_MODE_FIT;

m_lpAgoraObject->GetEngine()->setupLocalVideo(vc);
```

### 加入频道

现在你可以加入视频通话频道了。 调用 <code>joinChannel</code> 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。

-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。


> 如果已在通话中，则必须调用 <code>leaveChannel</code> 方法退出当前通话，才能进入下一个频道。

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
```

### 设置远端视频视图

远端视频视图，是指用户在本地设备上看到的远端视频流的视图。

在进入频道后，调用 <code>setupRemoteVideo</code> 方法设置本地看到的远端用户的视频视图。

在该方法中：

-   选择你想要看到的远端视频流的视窗。

-   指定你想要的视频显示模式。共有两种模式可选：Hidden 和 Fit 模式。两种模式下视频尺寸都是等比缩放，区别在于：

    -   Hidden 模式：优先保证视窗被填满。如果缩放后的视频尺寸与显示视窗尺寸不一致，多出的视频将被截掉。

    -   Fit 模式：优先保证视频内容全部显示。如果缩放后的视频尺寸与显示视窗尺寸不一致，未被填满的视窗区域将使用黑色填充。

-   传入你想要看到的远端视图的用户 UID。如果不知道想要指定的远端用户 UID，也可以在收到其他用户进入频道的回调事件 <code>onUserJoined</code> 中使用该方法。


```
VideoCanvas vc;

vc.renderMode = RENDER_MODE_FIT;
vc.uid = lpData->uid;
vc.view = m_wndRemote.GetSafeHwnd();

m_lpAgoraObject->GetEngine()->setupRemoteVideo(vc);
```

### 结束视频通话

视频通话结束时，调用 <code>leaveChannel</code> 方法离开频道。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。真正退出频道后，SDK 会触发 <code>onleaveChannel</code> 回调。

> 如果在调用 <code>leaveChannel</code> 方法后立即使用 <code>release</code>，则退出频道会被打断，SDK 也不会触发 <code>onleaveChannel</code> 回调。

```
int nRet = m_lpAgoraEngine->leaveChannel();
```

## 结论

现在你可以开始一个一对一或一对多视频通话了。

## 参考

在上述视频通话过程中，我们使用了 Agora SDK 提供的部分 API 功能：

-   创建 agora::IRtcEngine 对象：<code>create</code>

-   初始化对象：<code>initialize</code>

-   销毁 IRtcEngine 对象：<code>release</code>

-   打开视频模式：<code>enableVideo</code>

-   设置本地视频显示属性：<code>setupLocalVideo</code>

-   加入频道：<code>joinChannel</code>

-   设置远端视频显示属性：<code>setupRemoteVideo</code>

-   离开频道：<code>leaveChannel</code>


更多功能实现，请参考  [视频通话 API](https://docs.agora.io/cn/Video/API%20Reference/cpp/index.html) 中各 API 的功能及描述。


