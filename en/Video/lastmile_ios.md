
---
title: Conduct a Last Mile Test
description: Conduct a Pre-call Lastmile Test
platform: iOS,macOS
updatedAt: Wed Dec 05 2018 03:03:26 GMT+0000 (UTC)
---
# Conduct a Last Mile Test
## Introduction

The pre-call networkd quality test checks if the current network quality suits the audio bitrate or the target bitrate of the chosen video profile before a user joins a channel. Returned to the user by callbacks every two seconds, the test results are a network quality rating based on packet-loss rate and network jitter and reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 kbps; the video SDK adjust the actual bitrate against the chosen video profile.



## Implementation

```swift
//Swift:
// Call the enableLastmileTest function before joining a channel. 
agoraKit.enableLastmileTest()

// Register the callback
func rtcEngine(_ engine: AgoraRtcEngineKit, lastmileQuality quality: AgoraNetworkQuality) {
 		// Quality here refers to the detected network quality type. You can use it for the related logics. 
		// ⑴ You can choose to end the lastmile test in the callback. 
}

// ⑵ You can also choose to end the lastmile test at any other time. Before the test ends, the onLastmileQuality() callback can be triggered multiple times. 
agoraKit.disableLastmileTest()
```

```oc
//Objective-C
// Call the enableLastmileTest function before joining a channel. 
[agoraKit enableLastmileTest];

// Register the callback
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine lastmileQuality:(AgoraNetworkQuality)quality {
}

	// You can also choose to end the lastmile test at any other time. Before the test ends, the onLastmileQuality() callback can be returned multiple times. 
[agoraKit disableLastmileTest];
```

## API Reference

- [enableLastmileTest](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLastmileTest)
- [disableLastmileTest](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableLastmileTest)
- [rtcEngine:lastmileQuality:](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:)

## Considerations

- Only before joining a channel can you conduct a lastmile test.
- Chances are, the callback returns UNKNOWN the first time it is triggered. Still you can get the test result from the callbacks coming after it. 




