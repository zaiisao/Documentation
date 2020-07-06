
---
title: Image Enhancement
description: 
platform: iOS
updatedAt: Mon Jul 06 2020 07:12:59 GMT+0800 (CST)
---
# Image Enhancement
## Introduction

Agora provides an image enhancement API for users in social and entertainment scenarios to improve their appearance in a video call or live interactive video streaming. With this API, users can adjust settings such as the image contrast, brightness, sharpness, and red saturation, as shown in the following figure.

![](https://web-cdn.agora.io/docs-files/1553754533580)

## Implementation

Before proceeding, ensure that you have implemented basic real-time functions in your project. See [Start a  Call](../../en/Interactive%20Broadcast/start_call_ios.md) or [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_ios.md) for details.


Call the [`setBeautyEffectOptions`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setBeautyEffectOptions:options:) method to flexibly add image enhancement features.

This method has two parameters: 

- `enabled`: sets whether or not to enable image enhancement.
- `options`: sets the image enhancement options, including `lighteningContrastLevel` for adjusting the contrast level, `lightening` for adjusting the brightness level, `smoothness` for adjusting the sharpness level, and `redness` for adjusting the red saturation level.

### Sample code

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

We also provide open-source demo projects that implement image enhancement on GitHub. You can try the demo and view the source code.

- [OpenVideoCall-iOS](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-iOS) for Swift. Refer to the code in [`RoomViewController.swift`](https://github.com/AgoraIO/Basic-Video-Call/blob/master/Group-Video/OpenVideoCall-iOS/OpenVideoCall/RoomViewController.swift#L66).
- [OpenVideoCall-iOS-Objective-C](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-iOS-Objective-C) for Objective-C. Refer to the code in [`RoomViewController.m`](https://github.com/AgoraIO/Basic-Video-Call/blob/master/Group-Video/OpenVideoCall-iOS-Objective-C/OpenVideoCall/RoomViewController.m#L79).

## Considerations

- This API method has return values. If the method call fails, the return value is < 0.
- For low-end phones, enabling image enhancement affects the system performance. When the video resolution is set as 360p, 720p or higher, and the frame rate is set as 30 fps or 15 fps, do not enable image enhancement.
