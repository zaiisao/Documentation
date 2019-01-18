
---
title: Set the Camera Focus
description: 
platform: iOS
updatedAt: Mon Jan 14 2019 07:18:30 GMT+0000 (UTC)
---
# Set the Camera Focus
## Introduction

Camera exposure and focus are commonly used functions in video calls to enable high quality video capture.

Agora SDK provides a set of camera management methods on the Android platform, with which users can switch between the front and rear camera, set the camera zoom factor, set the exposure region and set the focus position.

* Camera exposure: Auto exposure is supported where users can maually set the exposure region.
* Camera focus: Auto face-focus and manual focus are supported.

## Implementation

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/ios_video.md) for details.

```swift
// swift
// Check if camera exposure is supported
let isSupported = agoraKit.isCameraExposurePositionSupported()

if isSupported {
    // Set the camera exposure at (50, 100)
    let point = CGPoint(x: 50, y: 100)
    agoraKit.setCameraExposurePosition(point)
}

// Check if auto face focus is supported and start focusing
let isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
agoraKit.setCameraAutoFocusFaceModeEnabled(isSupported)

// Check if manual focus is suppoeted and start focusing
let isSupported = agoraKit.isCameraFocusPositionInPreviewSupported()

if isSupported {
	// Set the camera focus at (50, 100)
	let point = CGPoint(x: 50, y: 100)
	agoraKit.setCameraFocusPositionInPreview(point)
}
```

```objective-c
// objective-c
// Check if camera exposure is supported
let isSuppported = [agoraKit isCameraExposurePositionSupported];

if (isSupported) {
    // Set the camera exposure at (50, 100)
    CGPoint *point = CGPointMake(50, 100);
    [agoraKit setCameraExposurePosition: point];
}

// Check if auto face focus is supported and start focusing
Bool isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
[agoraKit setCameraAutoFocusFaceModeEnabled: isSupported];

// Check if manual focus is supported and start focusing
let isSupported = [agoraKit isCameraFocusPositionInPreviewSupported];

if (isSupported) {
	// Set the camera focus at (50, 100)
	CGPoint *point = CGPointMake(50, 100);
	[agoraKit setCameraFocusPositionInPreview: point];
}
```

You can get the camera expousre and focus area in the `cameraExposureDidChangedToRect` and `cameraFocusDidChangedToRect` callbacks.

```swift
// swift
// The camera focus area is updated
// You can monitor the focus event and implement corrensponding logic
func rtcEngine(_ engine: AgoraRtcEngineKit, cameraFocusDidChangedTo rect: CGRect) {
}
// The camera exposure area is updated
// You can monitor the exposure event and implement corrensponding logic
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine cameraExposureDidChangedToRect:(CGRect)rect{
}
```

```objective-c
// objective-c
// The camera focus area is updated
// You can monitor the focus event and implement corrensponding logic
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine cameraFocusDidChangedToRect:(CGRect)rect {
}
// The camera exposure area is updated
// You can monitor the exposure event and implement corrensponding logic
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine cameraExposureDidChangedToRect:(CGRect)rect {
}
```

### API Reference

- [`isCameraExposurePositionSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraExposurePositionSupported)
- [`setCameraExposurePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraExposurePosition:)
- [`isCameraFocusPositionInPreviewSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraFocusPositionInPreviewSupported)
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraAutoFocusFaceModeSupported)
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraFocusPositionInPreview:)
- [`setCameraAutoFocusModeEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraAutoFocusFaceModeEnabled:)
- [`cameraFocusDidChangedToRect`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraFocusDidChangedToRect:)
- [`cameraExposureDidChangedToRect`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraExposureDidChangedToRect:)
