
---
title: Image Enhancement
description: 
platform: iOS
updatedAt: Tue Sep 24 2019 07:50:35 GMT+0800 (CST)
---
# Image Enhancement
## Introduction
Agora provides an image enhancement API for users in social and entertainment scenarios to improve their appearance in video calls or live broadcasts. With this API, users can adjust settings such as the image contrast, brightness, sharpness, and red saturation, as shown in the following figure.

![](https://web-cdn.agora.io/docs-files/1553754533580)

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md).

The Agora SDK provides the [setBeautyEffectOptions](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setBeautyEffectOptions:options:) method to enable developers to flexibly add image enhancement features.

This method has two parameters: 

- `enabled`: sets whether or not to enable image enhancement.
- `options`: sets the image enhancement options, including `lighteningContrastLevel` for adjusting the contrast level, `lightening` for adjusting the brightness level, `smoothness` for adjusting the sharpness level, and `redness` for adjusting the red saturation level.

```swift
//swift
let options = AgoraBeautyOptions()
options.lighteningContrastLevel = .normal
options.rednessLevel = 0
options.smoothnessLevel = 0
options.lighteningLevel = 0

agoraKit.setBeautyEffectOptions(true, options: options)
```

```objective-c 
//objective-c
AgoraBeautyOptions *options = [[AgoraBeautyOptions alloc] init];
options.lighteningContrastLevel = AgoraLighteningContrastNormal;
options.rednessLevel = 0;
options.smoothnessLevel = 0;
options.lighteningContrastLevel = 0;

[self.agoraKit setBeautyEffectOptions:YES options:options];
```

## Sample Code

Agora provides an open source sample code that implements image enhancement. You can go to the [OpenLive-iOS Github Repo](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-iOS) to download it.

## Considerations
- This API method has return values. If the method call fails, the return value is < 0.
- For low-end phones, enabling image enhancement affects the system performance. When the video resolution is set as 360P, 720P or higher, and the frame rate is set as 30 fps or 15fps, do not enable image enhancement.
