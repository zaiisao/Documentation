
---
title: Image Enhancement
description: 
platform: Web
updatedAt: Mon Jul 06 2020 07:46:00 GMT+0800 (CST)
---
# Image Enhancement
## Introduction

Agora provides an image enhancement API for users in social and entertainment scenarios to improve their appearance in a video call or live interactive video streaming. With this API, users can adjust settings such as the image contrast, brightness, sharpness, and red saturation, as shown in the following figure:

![img](https://web-cdn.agora.io/docs-files/1553753660177)

## Implementation

Before proceeding, ensure that you have implemented basic real-time functions in your project. See [Start a  Call](../../en/Video/start_call_web.md) or [Start Live Interactive Streaming](../../en/Video/start_live_web.md) for details.

Call [`setBeautyEffectOptions`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.stream.html#setbeautyeffectoptions) to flexibly add image enhancement features. This method is asynchronous, and must be called with `Promise` or `async`/`await` keywords.

<div class="alert info">To enable image enhancement immediately after creating a video stream, call this method in the <code>Client.on("stream-published")</code> callback.</div>

<div class="alert note">This method supports the following browsers:
  <li>Safari 12 or later</li>
<li>Chrome 65 or later</li>
<li>Firefox 70.0.1 or later</li></div>

This method has two parameters:

- `enabled`: sets whether or not to enable image enhancement.
- `options`: sets the image enhancement options, including `lighteningContrastLevel` for adjusting the contrast level, `lighteningLevel` for adjusting the brightness level, `smoothnessLevel` for adjusting the sharpness level, and `rednessLevel` for adjusting the red saturation level.

### Sample code

```javascript
var streamPublishedHandler = async function() {
    await localStream.setBeautyEffectOptions(true, {
        lighteningContrastLevel: 1,
        lighteningLevel: 0.7,
        smoothnessLevel: 0.5,
        rednessLevel: 0.1
    });
    client.off("stream-published", streamPublishedHandler);
}
client.on("stream-published", streamPublishedHandler);
```

## Considerations

- This function does not support mobile devices.
- If the dual-stream mode is enabled, the image enhancement options only apply to the high-video stream.
- To enable image enhancement for video streams created by `replaceTrack` or `addTrack`, call `setBeautyEffectOptions` after the method call of `replaceTrack` or `addTrack` succeeds.
- If image enhancement is enabled, you must call `stream.setBeautyEffectOptions(false)` to disable it before calling the following methods:
	- `leave`
	- `stop`
	- `replaceTrack`
	- `removeTrack`
	- `unpublish`
- The image enhancement function involves real-time compute-intensive processing. Though it is based on hardware acceleration, the processing has high GPU and CPU overheads. For low-end devices, enabling image enhancement affects the system performance. When the video resolution is set as 360p, 720p or higher, and the frame rate is set as 30 fps or 15 fps, do not enable image enhancement.
