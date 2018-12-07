
---
title: Set the Camera Focus
description: 
platform: iOS
updatedAt: Fri Dec 07 2018 18:46:31 GMT+0000 (UTC)
---
# Set the Camera Focus
## Introduction

The camera focus function is used in video calls to enable high-quality video capture.

The Agora SDK provides a set of camera management methods on iOS, with which users can switch between the front and rear cameras, set the camera zoom factor, or set the camera focus position.

## Implementation

Ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Video/ios_video.md).

```swift
// swift
// Check if auto-face focus is supported and start focusing.
let isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
agoraKit.setCameraAutoFocusFaceModeEnabled(isSupported)

// Check if manual focus is supported and start focusing.
let isSupported = agoraKit.isCameraFocusPositionInPreviewSupported()

if isSupported {
	// Set the camera focus at (50, 100).
	let point = CGPoint(x: 50, y: 100)
	agoraKit.setCameraFocusPositionInPreview(point)
}
```

```objective-c
// objective-c
// Check if auto-face focus is supported and start focusing.
Bool isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
[agoraKit setCameraAutoFocusFaceModeEnabled: isSupported];

// Check if manual focus is supported and start focusing.
let isSupported = [agoraKit isCameraFocusPositionInPreviewSupported];

if (isSupported) {
	// Set the camera focus at (50, 100).
	CGPoint *point = CGPointMake(50, 100);
	[agoraKit setCameraFocusPositionInPreview: point];
}
```

### API Methods

- [`isCameraFocusPositionInPreviewSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraFocusPositionInPreviewSupported)
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraAutoFocusFaceModeSupported)
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraFocusPositionInPreview:)
- [`setCameraAutoFocusModeEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraAutoFocusFaceModeEnabled:)
