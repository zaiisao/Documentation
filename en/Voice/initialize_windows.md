
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: Windows
updatedAt: Thu Dec 13 2018 23:27:33 GMT+0000 (UTC)
---
# Create and Initialize an AgoraRtcEngine Instance
Before creating and initializing the client, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Voice/windows_video.md).

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
You have created the client and can start a voice call with the following steps:

- [Join a Channel](../../en/Voice/join_communication_windows.md)
- [Publish and Subscribe to Streams](../../en/Voice/publish_windows_audio.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections:

- [Conduct a Last Mile Test](../../en/Voice/lastmile_windows.md)
- [Set the Stereo/High-fidelity Audio Profile](../../en/Voice/audio_profile_windows.md)
