
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: Windows
updatedAt: Tue Oct 22 2019 06:19:10 GMT+0800 (CST)
---
# Create and Initialize an AgoraRtcEngine Instance
## Prerequisites

Before creating and initializing the client, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/windows_video.md).

Create a project in Agora Console and get the App ID of the project. You need to pass in the App ID during initialization.

1. Sign up for a developer account at [Agora Console](https://dashboard.agora.io/). See [Sign in and Sign up](../../en/Video/sign_in_and_sign_up.md).

2. Click ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to enter the [**Project Management**](https://dashboard.agora.io/projects) page.

3. Click **Create**. 

![](https://web-cdn.agora.io/docs-files/1574924327108)

4.  Enter your project name and select your authentication mechanism ("App ID") in the dialog box.

![](https://web-cdn.agora.io/docs-files/1574924446798)
	
5. Click **Submit** and you can find the **App ID** of your newly created project.

![](https://web-cdn.agora.io/docs-files/1574924570426)

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

> Ensure that you call the `create` and `initialize` methods to intiialize the AgoraRtcEngine before calling any other API. 

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
</ul>
</td>
</tr>
</tbody>
</table>


### Code Snippets

`AParameter apm(*m_lpAgoraEngine);
 int nRet = apm->setInt("che.video.videoCaptureType", nType);`



