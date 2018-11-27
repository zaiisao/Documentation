
---
title: 摄像头对焦
description: 
platform: iOS
updatedAt: Tue Nov 27 2018 05:28:42 GMT+0000 (UTC)
---
# 摄像头对焦
## 功能简介

视频场景中，经常会使用到摄像头对焦的功能，帮助被拍摄物成像清晰。
Agora SDK 在 iOS 平台提供整套的摄像头管理方法，方便用户切换前后摄像头，以及对摄像头的缩放、对焦进行设置。

## 实现方法

在设置摄像头对焦前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Video/ios_video.md)。


```swift
//swift
//检测当前设备是否支持人脸自动对焦并设置
let isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
agoraKit. setCameraAutoFocusFaceModeEnabled(isSupported)

//检测当前设备是否支持手动对焦功能并设置
let isSupported = agoraKit.isCameraFocusPositionInPreviewSupported()

if isSupported {
	//假设在屏幕（50，100）的位置对焦
	let point = CGPoint(x: 50, y: 100)
	agoraKit.setCameraFocusPositionInPreview(point)
}
```

```objective-c
//objective-c
//检测当前设备是否支持人脸自动对焦并设置
Bool isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
[agoraKit setCameraAutoFocusFaceModeEnabled: isSupported];

//检测当前设备是否支持手动对焦功能并设置
let isSupported = [agoraKit isCameraFocusPositionInPreviewSupported];

if (isSupported) {
	//假设在屏幕（50，100）的位置对焦
	CGPoint *point = CGPointMake(50, 100);
	[agoraKit setCameraFocusPositionInPreview: point];
}
```

### API 参考

- [`isCameraFocusPositionInPreviewSupported`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraFocusPositionInPreviewSupported)
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraAutoFocusFaceModeSupported)
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraFocusPositionInPreview:)
- [`setCameraAutoFocusFaceModeEnabled`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraAutoFocusFaceModeEnabled:)

