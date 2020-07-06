
---
title: Image Enhancement
description: 
platform: Android
updatedAt: Mon Jul 06 2020 07:05:59 GMT+0800 (CST)
---
# Image Enhancement
## Introduction

Agora provides an image enhancement API for users in social and entertainment scenarios to improve their appearance in a video call or live interactive video streaming. With this API, users can adjust settings such as the image contrast, brightness, sharpness, and red saturation, as shown in the following figure:

![](https://web-cdn.agora.io/docs-files/1553753660177)

## Implementation

Before proceeding, ensure that you implement a basic call or live interactive streaming in your project. See [Start a Video Call](../../en/Interactive%20Broadcast/start_call_android.md) or [Start Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_android.md) for details.

Call the [`setBeautyEffectOptions`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa9327de4fb0c29f840b1e68ca2e83fc6) method to flexibly add image enhancement features.

This method has two parameters: 

- `enabled`: sets whether or not to enable image enhancement.
- `options`: sets the image enhancement options, including `lighteningContrastLevel` for adjusting the contrast level, `lightening` for adjusting the brightness level, `smoothness` for adjusting the sharpness level, and `redness` for adjusting the red saturation level.

### Sample code

```java
mRtcEngine.setBeautyEffectOptions(true, new BeautyOptions(LIGHTENING_CONTRAST_NORMAL, 0.5F, 0.5F, 0.5F));
```

We also provide an open-source [OpenLive-Android](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-Android) demo project that implements image enhancement on GitHub. You can go to the repository to download it. You can try the demo and refer to the source code of the `onBeautyClicked` method in the  [`LiveActivity.java`](https://github.com/AgoraIO/Basic-Video-Broadcasting/blob/master/OpenLive-Android/app/src/main/java/io/agora/openlive/activities/LiveActivity.java) file.

## Considerations
- This API method has return values. If the method call fails, the return value is < 0.
- For low-end phones, enabling image enhancement affects the system performance. When the video resolution is set as 360p, 720p or higher, and the frame rate is set as 30 fps or 15 fps, do not enable image enhancement.
