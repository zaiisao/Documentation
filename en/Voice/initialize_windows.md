
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: Windows
updatedAt: Tue Dec 11 2018 07:30:34 GMT+0000 (UTC)
---
# Create and Initialize an AgoraRtcEngine Instance
Before creating and initizing the Client, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/windows_video.md) for more information.

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
You hava now finished creating the Client and can start a voice call with the following steps:

- [Join a Channel](../../en/Voice/join_communication_windows.md)
- [Publish and Subscribe to Streams](../../en/Voice/publish_windows_audio.md)

For added requirements on network connection or audio quality, you can also take the following steps before joining a channel.

- [Conduct a Last Mile Test](../../en/Voice/lastmile_windows.md)
- [Set the Stereo/High-fidelity Audio Profile](../../en/Voice/audio_profile_windows.md)
