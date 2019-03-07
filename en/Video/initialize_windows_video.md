
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: Windows
updatedAt: Thu Mar 07 2019 14:35:32 GMT+0000 (UTC)
---
# Create and Initialize an AgoraRtcEngine Instance
Before creating and initializing the client, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/windows_video.md).

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
You have created the client and can start a video call with the following steps:

- [Join a Channel](../../en/Video/join_video_windows.md)
- [Publish and Subscribe to Streams](../../en/Video/publish_windows.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections.

- [Conduct a Last Mile Test](../../en/Video/lastmile_windows.md)
- [Set the Stereo/High-fidelity Audio Profile](../../en/Video/audio_profile_windows.md)

## Troubleshooting

Media Foundation and DirectShow are two video-capture architectures for Microsoft Windows. Some cameras support both, while some do not. This may cause no-video from the capturer. Agora allows you to use its private interface to select your video capture architecture. 

### Private Interfaces

`agora::rtc::AParameter::AParameter::setInt(const char* key, int value);
`

### Parameter Descriptions

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>key</code></td>
<td>"che.video.videoCaptureType"</td>
</tr>
<tr><td><code>value</code></td>
<ul>
<li>0: DirectShow</li>
<li>1: VFW (Video for Windows)</li>
<li>2: Media Foundation</li>
<li>3: DirectShow</li>
</ul>
</td>
</tr>
</tbody>
</table>


### Code Snippets

`AParameter apm(*m_lpAgoraEngine);
 int nRet = apm->setInt("che.video.videoCaptureType", nType);`



