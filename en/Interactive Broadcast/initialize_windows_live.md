
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: Windows
updatedAt: Thu Mar 07 2019 07:27:35 GMT+0000 (UTC)
---
# Create and Initialize an AgoraRtcEngine Instance
Before creating and initializing the AgoraRtcEngine instance, ensure that you  prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/windows_video.md).

## Implementation

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


## Next Steps
You have created the AgoraRtcEngine instance and can start a live broadcast with the following steps:
* [Join a Channel](../../en/Interactive%20Broadcast/join_live_windows.md)
* [Switch the Client Role](../../en/Interactive%20Broadcast/role_windows.md)
* [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_windows_live.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections:

* [Conduct a Last Mile Test](../../en/Interactive%20Broadcast/lastmile_windows.md)
* [Set the Stereo/High-fidelity Audio Profile](../../en/Interactive%20Broadcast/audio_profile_windows.md)
