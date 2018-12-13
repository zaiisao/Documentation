
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: Windows
updatedAt: Thu Dec 13 2018 16:15:54 GMT+0000 (UTC)
---
# Create and Initialize an AgoraRtcEngine Instance
Before creating and initizing the AgoraRtcEngine instance, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/windows_video.md) for more information.

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
You have now finished creating the AgoraRtcEngine instance and can start a live broadcast with the following steps:
* [Join a Channel](../../en/Interactive%20Broadcast/join_live_windows.md)
* [Switch the Client Role](../../en/Interactive%20Broadcast/role_windows.md)
* [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_windows_live.md)

For added requirements on network connection or audio quality, you can also take the following steps before joining a channel:

* [Conduct a Last mile Test](../../en/Interactive%20Broadcast/lastmile_windows.md)
* [Set the Stereo/High-fidelity Audio Profile](../../en/Interactive%20Broadcast/audio_profile_windows.md)
