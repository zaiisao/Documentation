
---
title: Image Enhancement
description: 
platform: Web
updatedAt: Mon Mar 09 2020 06:35:55 GMT+0800 (CST)
---
# Image Enhancement
## Introduction

Agora provides an image enhancement API for users in social and entertainment scenarios to improve their appearance in video calls or live broadcasts. With this API, users can adjust settings such as the image contrast, brightness, sharpness, and red saturation, as shown in the following figure:

![img](https://web-cdn.agora.io/docs-files/1553753660177)

## Implementation

Before proceeding, ensure that you implement a basic call or live broadcast in your project. See [Start a Call](../../en/Video/start_call_web.md) or [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_web.md) for details.

Call the [`setBeautyEffectOptions`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.stream.html#setbeautyeffectoptions) method to flexibly add image enhancement features.

<div class="alert note">This method supports the following browsers:
  <li>Safari 12 or later</li>
<li>Chrome 65 or later</li>
<li>Firefox 70.0.1 or later</li></div>

This method has two parameters:

- `enabled`: sets whether or not to enable image enhancement.
- `options`: sets the image enhancement options, including `lighteningContrastLevel` for adjusting the contrast level, `lighteningLevel` for adjusting the brightness level, `smoothnessLevel` for adjusting the sharpness level, and `rednessLevel` for adjusting the red saturation level.

### Sample code

```javascript
stream.setBeautyEffectOptions(true, {
    lighteningContrastLevel: 1,
    lighteningLevel: 0.7,
    smoothnessLevel: 0.5,
    rednessLevel: 0.1
});
```

We also provide an open-source [OpenLive-Web](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-Web) sample project that implements image enhancement on GitHub. You can try it [online](https://webdemo.agora.io/agora-web-showcase/examples/OpenLive-Web/#/) and refer to the source code in the [`rtc-client.js`](https://github.com/AgoraIO/Basic-Video-Broadcasting/blob/master/OpenLive-Web/src/rtc-client.js#L82) file.

## Considerations

- This function does not support mobile devices.
- If the dual-stream mode is enabled, the image enhancement options only apply to the high-video stream.
- To remove a video track after enabling image enhancement, you need to first disable the enhancement by calling `setBeautyEffectOptions`.
- For low-end devices, enabling image enhancement affects the system performance. When the video resolution is set as 360p, 720p or higher, and the frame rate is set as 30 fps or 15 fps, do not enable image enhancement.
