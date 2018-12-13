
---
title: 创建 AgoraRtcEngine 实例并初始化
description: windows平台初始化
platform: Windows
updatedAt: Thu Dec 13 2018 07:57:01 GMT+0000 (UTC)
---
# 创建 AgoraRtcEngine 实例并初始化
在创建 AgoraRtcEngine 实例并初始化前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Interactive%20Broadcast/windows_video.md)。

## 实现方法
进入频道之前，调用 <code>create</code> 方法创建一个 AgoraRtcEngine 实例，并调用 <code>initialize</code> 方法完成初始化。

填入获取到的 App ID 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

## 相关文档
完成创建实例后，你可以使用 Agora SDK，依次实现如下功能进行互动直播：
* [加入频道](../../cn/Interactive%20Broadcast/join_live_windows.md)
* [切换用户角色](../../cn/Interactive%20Broadcast/role_windows.md)
* [发布和订阅音频流](../../cn/Interactive%20Broadcast/publish_windows_live.md)

如果对网络或音质有特殊的需求，你还可以在加入频道前：
* [进行通话前网络质量监测](../../cn/Interactive%20Broadcast/lastmile_windows.md)
* [使用双声道/高音质](../../cn/Interactive%20Broadcast/audio_profile_windows.md)

