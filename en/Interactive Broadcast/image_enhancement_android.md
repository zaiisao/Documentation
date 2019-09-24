
---
title: Image Enhancement
description: 
platform: Android
updatedAt: Tue Sep 24 2019 07:10:57 GMT+0800 (CST)
---
# Image Enhancement
## Introduction
Agora provides an image enhancement API for users in social and entertainment scenarios to improve their appearance in video calls or live broadcasts. With this API, users can adjust settings such as the image contrast, brightness, sharpness, and red saturation, as shown in the following figure:

![](https://web-cdn.agora.io/docs-files/1553753660177)

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/android_video.md).

The Agora SDK provides the [setBeautyEffectOptions](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/v2.4/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa9327de4fb0c29f840b1e68ca2e83fc6) method to enable developers to flexibly add image enhancement features.

This method has two parameters: 

- `enabled`: sets whether or not to enable image enhancement.
- `options`: sets the image enhancement options, including `lighteningContrastLevel` for adjusting the contrast level, `lightening` for adjusting the brightness level, `smoothness` for adjusting the sharpness level, and `redness` for adjusting the red saturation level.

```java
mRtcEngine.setBeautyEffectOptions(true, new BeautyOptions(LIGHTENING_CONTRAST_NORMAL, 0.5F, 0.5F, 0.5F));
```

## Sample Code

Agora provides an open source sample code that implements image enhancement. You can go to the [OpenLive-Android Github Repo](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-Android) to download it.

## Considerations
- This API method has return values. If the method call fails, the return value is < 0.
- For low-end phones, enabling image enhancement affects the system performance. When the video resolution is set as 360P, 720P or higher, and the frame rate is set as  30 fps or 15 fps, do not enable image enhancement.
