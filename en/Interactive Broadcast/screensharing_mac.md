
---
title: Share the screen
description: 
platform: macOS
updatedAt: Tue Nov 27 2018 06:25:25 GMT+0000 (UTC)
---
# Share the screen
## Introduction

During a video call or live broadcast, **sharing the screen** enhances communication by bringing whatever is on the speaker's screen to the other speakers or audience in the channel.

Screen share has extensive application in the following scenarios:

- For a video conference, the speaker can share the image of the local file, web page, and PPT with other users in the channel.
- For an online class, the teacher can share the image of the slides or notes with the students.

## Implementations

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/mac_video.md) for details.

Screen sharing on the macOS platform is implemented with the following steps:

- Get the `windowId` of the view. If the `windowId` is 0, it means to share the whole screen.
- Switch the video source from camera to view and transfer the video frame to the remote user.

```swift
//swift
//start sharing the screen
let windowId = 0
let captureFreq = 15
let bitRate = 400
let rect = CGRect.zero
agoraKit.startScreenCapture(windowId, withCaptureFreq: captureFreq, bitRate: bitRate, andRect: rect)


// Update the screen region to be shared
let rect = CGRect(x: 0, y: 0, width: 100, height: 100)
agoraKit.updateScreenCaptureRegion(rect)

//stop sharing the screen
agoraKit.stopScreenCapture()
```

```objective-c
//objective-c
int windowId = 0;
int captureFreq = 15;
int bitRate = 400;
CGRect rect = CGRectZero;

// Update the screen region to be shared
CGRect rect = CGRectMake(0, 0, 100, 100);
[agoraKit startScreenCapture: windowId withCaptureFreq: captureFreq bitRate:(NSInteger)bitRate andRect: rect]; 

//stop sharing the screen
[agoraKit stopScreenCapture];
```

**Relevant APIs and descriptions**
* [`startScreenCapture:withCaptureFreq:bitrRate:andRect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCapture:withCaptureFreq:bitRate:andRect:)
* [`stopScreenCapture`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopScreenCapture)
* [`updateScreenCaptureRegion:`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureRegion:)
