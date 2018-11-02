
---
title: 创建 AgoraRtcEngine 实例并初始化
description: windows平台初始化
platform: Windows
updatedAt: Thu Nov 01 2018 08:05:06 GMT+0000 (UTC)
---
# 创建 AgoraRtcEngine 实例并初始化
# 创建 AgoraRtcEngine 实例并初始化

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

