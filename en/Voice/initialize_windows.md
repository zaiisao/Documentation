
---
title: Create and Initialize AgoraRtcEngine Instance
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 04:07:47 GMT+0000 (UTC)
---
# Create and Initialize AgoraRtcEngine Instance
Create an AgoraRtcEngine instance by invoking <code>create</code>, and initialize the instance by invoking <code>initialize</code>.

Pass the Agora App ID. Only applications with the same App ID can join the same channel.

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

