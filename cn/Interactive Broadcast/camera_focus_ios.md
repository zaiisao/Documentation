
---
title: 摄像头对焦
description: 
platform: iOS
updatedAt: Tue Sep 17 2019 10:26:26 GMT+0800 (CST)
---
# 摄像头对焦
## 功能简介

视频场景中，经常会使用到摄像头曝光和对焦的功能，帮助被拍摄物成像清晰、亮度适宜。
Agora SDK 在 iOS 平台提供整套的摄像头管理方法，方便用户切换前后摄像头，并对摄像头的缩放、对焦和曝光进行设置。

曝光：支持手动选择曝光区域，摄像头自动曝光
对焦：支持人脸自动对焦、手动对焦

## 实现方法

在设置摄像头对焦前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Interactive%20Broadcast/ios_video.md)。


```swift
// swift
// 检测当前设备是否支持自动曝光并设置
let isSupported = agoraKit.isCameraExposurePositionSupported()

if isSupported {
    // 假设在屏幕(50，100)的位置曝光
    let point = CGPoint(x: 50, y: 100)
    agoraKit.setCameraExposurePosition(point)
}

// 检测当前设备是否支持人脸自动对焦并设置
let isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
agoraKit.setCameraAutoFocusFaceModeEnabled(isSupported)

// 检测当前设备是否支持手动对焦功能并设置
let isSupported = agoraKit.isCameraFocusPositionInPreviewSupported()

if isSupported {
	// 假设在屏幕（50，100）的位置对焦
	let point = CGPoint(x: 50, y: 100)
	agoraKit.setCameraFocusPositionInPreview(point)
}
```

```objective-c
// objective-c
// 检测当前设备是否支持自动曝光并设置
let isSuppported = [agoraKit isCameraExposurePositionSupported];

if (isSupported) {
    // 假设在屏幕(50，100)的位置曝光
    CGPoint *point = CGPointMake(50, 100);
    [agoraKit setCameraExposurePosition: point];
}

// 检测当前设备是否支持人脸自动对焦并设置
Bool isSupported = [agoraKit isCameraAutoFocusFaceModeSupported];
[agoraKit setCameraAutoFocusFaceModeEnabled: isSupported];

// 检测当前设备是否支持手动对焦功能并设置
let isSupported = [agoraKit isCameraFocusPositionInPreviewSupported];

if (isSupported) {
	// 假设在屏幕（50，100）的位置对焦
	CGPoint *point = CGPointMake(50, 100);
	[agoraKit setCameraFocusPositionInPreview: point];
}
```

如果想获取摄像头的曝光和对焦位置，也可以在 `cameraExposureDidChangedToRect` 和  `cameraFocusDidChangedToRect` 回调中实现。

```swift
// swift
// 对焦区域更新
// 你可以在这里监听到焦点更新事件，并进行自己的逻辑处理
func rtcEngine(_ engine: AgoraRtcEngineKit, cameraFocusDidChangedToRect: CGRect) {
}
// 曝光区域更新
// 你可以在这里监听到曝光更新事件，并进行自己的逻辑处理
func rtcEngine(_ engine: AgoraRtcEngineKit, cameraExposureDidChangedToRect: CGRect) {
}
```

```objective-c
// objective-c
// 对焦区域更新
// 你可以在这里监听到焦点更新事件，并进行自己的逻辑处理
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine cameraFocusDidChangedToRect:(CGRect)rect {
}
// 曝光区域更新
// 你可以在这里监听到曝光更新事件，并进行自己的逻辑处理
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine cameraExposureDidChangedToRect:(CGRect)rect {
}
```


### API 参考

- [`isCameraExposurePositionSupported`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraExposurePositionSupported)
- [`setCameraExposurePosition`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraExposurePosition:)
- [`isCameraFocusPositionInPreviewSupported`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraFocusPositionInPreviewSupported)
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraAutoFocusFaceModeSupported)
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraFocusPositionInPreview:)
- [`setCameraAutoFocusModeEnabled`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraAutoFocusFaceModeEnabled:)
- [`cameraFocusDidChangedToRect`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraFocusDidChangedToRect:)
- [`cameraExposureDidChangedToRect`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraExposureDidChangedToRect:)
