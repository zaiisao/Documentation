
---
title: Conduct a Last-mile Test
description: Conduct a Pre-call Lastmile Test
platform: iOS,macOS
updatedAt: Tue Feb 19 2019 09:48:55 GMT+0800 (CST)
---
# Conduct a Last-mile Test
## Introduction

To check if the uplink network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before joining the channel.

This function particularly applies to scenarios which have high requirements on the network quality.

> The audio SDK uses a fixed bitrate of 48 Kbps. The video SDK adjusts the actual bitrate according to the chosen video profile.



## Implementation

```swift
//Swift:
// Call the enableLastmileTest method before joining a channel. 
agoraKit.enableLastmileTest()

// Register the callback.
func rtcEngine(_ engine: AgoraRtcEngineKit, lastmileQuality quality: AgoraNetworkQuality) {
 		// quality means the detected network quality type. You can use it for the related logic. 
		// ⑴ You can choose to end the last mile test in the callback. 
}

// ⑵ You can also choose to end the last mile test at any time. Before the test ends, the onLastmileQuality callback can be triggered multiple times. 
agoraKit.disableLastmileTest()
```

```oc
//Objective-C
// Call the enableLastmileTest method before joining a channel. 
[agoraKit enableLastmileTest];

// Register the callback.
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine lastmileQuality:(AgoraNetworkQuality)quality {
}

	// You can also choose to end the last mile test at any time. Before the test ends, the onLastmileQuality callback can be returned multiple times. 
[agoraKit disableLastmileTest];
```

### API Reference

- [`enableLastmileTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLastmileTest)
- [`disableLastmileTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableLastmileTest)
- [`rtcEngine:lastmileQuality:`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:)

## Considerations

- You can conduct a last mile test only before joining a channel. Before the test ends, Agora recommends not calling any other API methods.
- The `onLastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 




