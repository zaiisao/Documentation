
---
title: Set the Camera Focus
description: 
platform: iOS
updatedAt: Tue Nov 27 2018 01:30:07 GMT+0000 (UTC)
---
# Set the Camera Focus
## Introduction

Camera focus is a commonly used function in video calls to enable high quality video capture.

Agora SDK provides a set of camera management methods on the Android platform, with which users can switch between the front and rear camera, set the camera zoom factor or set the focus position.

## Implementations

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/ios_video.md) for details.

```swift
//swift
// Check if auto face focus is supported and start focusing
let isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
agoraKit. setCameraAutoFocusFaceModeEnabled(isSupported)

// Check if manual focus is suppoeted and start focusing
let isSupported = agoraKit.isCameraFocusPositionInPreviewSupported()

if isSupported {
	let point = CGPoint(x: 50, y: 50)
	agoraKit.setCameraFocusPositionInPreview(point)
}
```

```objective-c
//objective-c
// Check if auto face focus is supported and start focusing
Bool isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
[agoraKit setCameraAutoFocusFaceModeEnabled: isSupported];

// Check if manual focus is supported and start focusing
let isSupported = [agoraKit isCameraFocusPositionInPreviewSupported];

if (isSupported) {
	CGPoint *point = CGPointMake(50, 50);
	[agoraKit setCameraFocusPositionInPreview: point];
}
```

**Relevant APIs and descriptions**

-  [`isCameraFocusPositionInPreviewSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraFocusPositionInPreviewSupported): Checks whether the camera zoom function is supported.
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraAutoFocusFaceModeSupported): Checks whether the camera auto-face focus function is supported.
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraFocusPositionInPreview:): Sets the camera manual focus position.
- [`setCameraAutoFocusModeEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraAutoFocusFaceModeEnabled:): Enables the camera auto-face focus function.
