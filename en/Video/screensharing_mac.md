
---
title: Share the screen
description: 
platform: macOS
updatedAt: Fri Mar 29 2019 02:49:54 GMT+0000 (UTC)
---
# Share the screen
## Introduction

During a video call or live broadcast, **sharing the screen** enhances communication by displaying the speaker's screen on the display of other speakers or audience members in the channel.

Screen sharing is applied in the following scenarios:

- In a video conference, the speaker can share an image of a local file, web page, or presentation with other users in the channel.
- In an online class, the teacher can share the slides or notes with students.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/mac_video.md).

Screen sharing on macOS is implemented with the following steps:

- Get the `windowId` of the view. `windowId` = 0 means sharing the whole screen.
- Switch the video source from the camera to the view and transfer the video frame to the remote user.

```swift
// swift
// Start sharing the screen.
let windowId = 0
let captureFreq = 15
let bitRate = 400
let rect = CGRect.zero
agoraKit.startScreenCapture(windowId, withCaptureFreq: captureFreq, bitRate: bitRate, andRect: rect)


// Update the screen region to be shared.
let rect = CGRect(x: 0, y: 0, width: 100, height: 100)
agoraKit.updateScreenCaptureRegion(rect)

// Stop sharing the screen.
agoraKit.stopScreenCapture()
```

```objective-c
// objective-c
int windowId = 0;
int captureFreq = 15;
int bitRate = 400;
CGRect rect = CGRectZero;

// Update the screen region to be shared.
CGRect rect = CGRectMake(0, 0, 100, 100);
[agoraKit startScreenCapture: windowId withCaptureFreq: captureFreq bitRate:(NSInteger)bitRate andRect: rect]; 

// Stop sharing the screen.
[agoraKit stopScreenCapture];
```

### API Reference
* [`startScreenCapture:withCaptureFreq:bitrRate:andRect`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCapture:withCaptureFreq:bitRate:andRect:)
* [`stopScreenCapture`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopScreenCapture)
* [`updateScreenCaptureRegion:`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureRegion:)
