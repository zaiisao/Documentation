
---
title: Camera Exposure and Focus
description: 
platform: iOS
updatedAt: Mon Jul 06 2020 08:19:41 GMT+0800 (CST)
---
# Camera Exposure and Focus
## Introduction

Camera exposure and focus are commonly used in video calls to enable high-quality video capture. Agora SDK provides a set of camera management methods on the iOS platform, with which you can switch between the front and rear camera, set the camera zoom factor, set the exposure region and set the focus position.

- Camera exposure: Auto exposure is supported where users can maually set the exposure region.
- Camera focus: Auto face-focus and manual focus are supported.

## Implementation

Before proceeding, ensure that you have implemented basic real-time functions in your project. See [Start a  Call](../../en/Video/start_call_ios.md) or [Start Live Interactive Streaming](../../en/Video/start_live_ios.md) for details.

Refer to the following steps to set the camera exposure and focus:

- **Exposure**
Call the `isCameraExposurePositionSupported` method to check whether exposure function is supported. If it is supported, call the `setCameraExposurePosition` method to set the camera exposure.
You can monitor the exposure position with the `cameraFocusDidChangedToRect` callback.
- **Auto face-focus**
Call the `isCameraAutoFocusFaceModeSupported` method to check whether auto face-focus function is supported. If it is supported, call the `setCameraAutoFocusModeEnabled` method to set the focus.
- **Manual focus**
Call the `isCameraFocusPositionInPreviewSupported` method to check whether manual focus function is supported. If it is supported, call the `setCameraFocusPositionInPreview` method to set the focus.
You can monitor the focus position with the `cameraExposureDidChangedToRect` callback.

### Sample code

```swift
//Swift
// Check whether exposure function is supported and enable exposure.
  let isSupported = agoraKit.isCameraExposurePositionSupported()

if isSupported {
    // Set the camera exposure at (50, 100).
    let point = CGPoint(x: 50, y: 100)
    agoraKit.setCameraExposurePosition(point)
}

// Check whether auto face-focus function is supported and enable auto-face focus.
let isSupported = agoraKit.isCameraAutoFocusFaceModeSupported()
agoraKit.setCameraAutoFocusFaceModeEnabled(isSupported)

// Check whether manual focus function is supported and set the focus.
let isSupported = agoraKit.isCameraFocusPositionInPreviewSupported()

if isSupported {
    // Set the camera focus at (50, 100).
    let point = CGPoint(x: 50, y: 100)
    agoraKit.setCameraFocusPositionInPreview(point)
}


// The camera focus area is updated. You can monitor the update event and implement corresponding logic.
func rtcEngine(_ engine: AgoraRtcEngineKit, cameraExposureDidChangedToRect: CGRect) {
}

// The camera exposure area is updated. You can monitor the update event the implement corresponding logic.
func rtcEngine(_ engine: AgoraRtcEngineKit, cameraFocusDidChangedToRect: CGRect) {
}
```

```objective-c
//Objective-C
// Check whether exposure function is supported and enable exposure.
let isSuppported = [agoraKit isCameraExposurePositionSupported];

if (isSupported) {
    // Set the camera exposure at (50, 100).
    CGPoint *point = CGPointMake(50, 100);
    [agoraKit setCameraExposurePosition: point];
}

// Check whether auto face-focus function is supported and enable auto-face focus.
Bool isSupported = [agoraKit isCameraAutoFocusFaceModeSupported];
[agoraKit setCameraAutoFocusFaceModeEnabled: isSupported];

// Check whether manual focus function is supported and set the focus.
let isSupported = [agoraKit isCameraFocusPositionInPreviewSupported];

if (isSupported) {
    // Set the camera focus at (50, 100).
    CGPoint *point = CGPointMake(50, 100);
    [agoraKit setCameraFocusPositionInPreview: point];
}


// The camera focus area is updated. You can monitor the update event and implement corresponding logic.
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine cameraExposureDidChangedToRect:(CGRect)rect {
}

// The camera exposure area is updated. You can monitor the update event the implement corresponding logic.
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine cameraFocusDidChangedToRect:(CGRect)rect {
}
```

### API reference

- [`isCameraExposurePositionSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraExposurePositionSupported)
- [`setCameraExposurePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraExposurePosition:)
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraAutoFocusFaceModeSupported)
- [`setCameraAutoFocusModeEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraAutoFocusFaceModeEnabled:)
- [`isCameraFocusPositionInPreviewSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraFocusPositionInPreviewSupported)
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraFocusPositionInPreview:)
- [`cameraExposureDidChangedToRect`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraExposureDidChangedToRect:)
- [`cameraFocusDidChangedToRect`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraFocusDidChangedToRect:)
