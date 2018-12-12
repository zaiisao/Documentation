
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: Windows
updatedAt: Wed Dec 12 2018 07:18:44 GMT+0000 (UTC)
---
# Create and Initialize an AgoraRtcEngine Instance
Before creating and initizing the Client, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/windows_video.md) for more information.

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
You hava now finished creating the Client and can start a video call with the following steps:

- [Join a Channel](../../en/Video/join_video_windows.md)
- [Publish and Subscribe to Streams](../../en/Video/publish_windows.md)

For added requirements on network connection or audio quality, you can also take the following steps before joining a channel.

- [Conduct a Last Mile Test](../../en/Video/lastmile_windows.md)
- [Set the Stereo/High-fidelity Audio Profile](../../en/Video/audio_profile_windows.md)
