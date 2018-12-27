
---
title: Conduct a Last Mile Test
description: Conduct a Pre-call Lastmile Test
platform: iOS,macOS
updatedAt: Thu Dec 27 2018 02:46:22 GMT+0000 (UTC)
---
# Conduct a Last Mile Test
## Introduction

You can conduct a last mile network quality test before starting a call to check if the network supports the audio bitrate or target bitrate of the chosen video profile before a user joins a channel. The `onLastmileQuality` callback reports the test results once every two seconds. The test results are based on the network quality ratings determined by the packet-loss rates and network jitter, which reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 Kbps. 
> The video SDK adjusts the actual bitrate according to the chosen video profile.



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

- You can conduct a last mile test only before joining a channel.
- The `onLastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 




