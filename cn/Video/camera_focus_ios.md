
---
title: 摄像头对焦
description: 
platform: iOS
updatedAt: Tue Nov 20 2018 08:16:52 GMT+0000 (UTC)
---
# 摄像头对焦
## 功能简介

视频场景中，经常会使用到摄像头对焦的功能，帮助被拍摄物成像清晰。
Agora SDK 在 iOS 平台提供整套的摄像头管理方法，方便用户切换前后摄像头，以及对摄像头的缩放、对焦进行设置。

## 实现方法

在设置摄像头对焦前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Video/ios_video.md)。


```swift
//swift
// 检测当前设备是否支持人脸自动对焦并设置
let isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
agoraKit. setCameraAutoFocusFaceModeEnabled(isSupported)

// 检测设备是否支持手动对焦功能并设置
let isSupported = agoraKit.isCameraFocusPositionInPreviewSupported()

if isSupported {
	let point = CGPoint(x: 50, y: 50)
	agoraKit.setCameraFocusPositionInPreview(point)
}
```

```objective-c
//objective-c
// 检测当前设备是否支持人脸自动对焦并设置
Bool isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
[agoraKit setCameraAutoFocusFaceModeEnabled: isSupported];

// 检测设备是否支持手动对焦功能并设置
let isSupported = [agoraKit isCameraFocusPositionInPreviewSupported];

if (isSupported) {
	CGPoint *point = CGPointMake(50, 50);
	[agoraKit setCameraFocusPositionInPreview: point];
}
```

**相关 API 及链接**

- [`isCameraFocusPositionInPreviewSupported`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraFocusPositionInPreviewSupported)：检测设备是否支持手动对焦功能
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraAutoFocusFaceModeSupported)：检测设备是否支持人脸对焦功能
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraFocusPositionInPreview:)：设置手动对焦位置，并触发对焦
- [`setCameraAutoFocusFaceModeEnabled`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraAutoFocusFaceModeEnabled:)：设置是否开启人脸对焦功能

