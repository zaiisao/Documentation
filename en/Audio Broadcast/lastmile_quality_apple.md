
---
title: Lastmile Tests
description: Conduct a Pre-call Lastmile Test
platform: iOS,macOS
updatedAt: Mon Jul 06 2020 03:37:55 GMT+0800 (CST)
---
# Lastmile Tests
## Introduction

To check if the uplink network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before joining the channel.

This function particularly applies to scenarios which have high requirements on the network quality.



## Implementation

Before conducting the last-mile test, ensure that you have implemented the basic real-time communication functions in your project. For details, see the following documents:
- iOS: [Start a Call](../../en/Audio%20Broadcast/start_call_ios.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_ios.md)
- macOS: [Start a Call](../../en/Audio%20Broadcast/start_call_mac.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_mac.md)

1. Call the `startLastmileProbeTest` method before joining a channel to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).
2. Once this method is enabled, the SDK triggers the following callbacks:
- `lastmileQuality`: Triggered two seconds after the `startLastmileProbeTest` method is called. This callback rates the network conditions with a score and is more closely linked to the user experience.
- `lastmileProbeResult`: Triggered 30 seconds after the `startLastmileProbeTest` method is called. This callback returns the real-time statistics of the network conditions and is more objective.
3. Call the `stopLastmileProbeTest` method to stop the last-mile network probe test.

### API call sequence

Refer to the following diagram to implement the last-mile test in your project.

![](https://web-cdn.agora.io/docs-files/1569465569670)

### Sample code

Refer to the following code to implement the last-mile test in your project.

```swift
// Swift
//  Configure a LastmileProbeConfig instance.
let config = AgoraLastmileProbeConfig(
  //Probe the uplink network quality.
  probeUplink: true, 
  // Probe the downlink network quality.
  probeDownlink: true,
  // The expected uplink bitrate (bps). The value range is [100000, 5000000].
  expectedUplinkBitrate: 100000, 
  // The expected downlink bitrate (bps). The value range is [100000, 5000000].
  expectedDownlinkBitrate: 100000)
// Start the last-mile network test before joining the channel.
agoraKit.startLastmileProbeTest(config)

// Register the callback events.
// Triggered 2 seconds after starting the last-mile test.
// quality means the detected network quality type.
func rtcEngine(_ engine: AgoraRtcEngineKit, lastmileQuality quality: AgoraNetworkQuality) {
}

// Triggered 30 seconds after starting the last-mile test.
// result means the last-mile probe test result.
func rtcEngien(_ engine: AgoraRtcEngineKit, lastmileProbeResult result: AgoraLastmileProbeResult){
  // (1) Stop the test. Agora recommends not calling any other API method before the test ends.
  agoraKit.stopLastmileProbeTest()  
}

// (2) Stop the test. Agora recommends not calling any other API method before the test ends.
agoraKit.stopLastmileProbeTest()
```

```objective-c
// Objective-C
//  Configure a LastmileProbeConfig instance.
AgoraLastmileProbeConfig *config = [[AgoraLastmileProbeConfig alloc] probeUplink: YES probeDownlink: YES expectedUplinkBitrate: 100000 expectedDownlinkBitrate: 100000];
// Start the last-mile network test before joining the channel.
[agoraKit startLastmileProbeTest: config];

// Register the callback events.
// Triggered 2 seconds after starting the last-mile test.
// quality means the detected network quality type.
- (void)rtcEngine:(AgoraRtcEngine * _Nonnull)engine lastmileQuality: (AgoraNetworkQuality)quality {
}

// Triggered 30 seconds after starting the last-mile test.
// result means the last-mile probe test result.
- (void)rtcEngine:(AgoraRtcEngine * _Nonnull)engine lastmileProbeResult: (AgoraLastmileProbeResult)result {
  // (1) Stop the test. Agora recommends not calling any other API method before the test ends.
  [agoraKit stopLastmileProbeTest];
}

// (2) Stop the test in an alternate place. Agora recommends not calling any other API method before the test ends.
[agoraKit stopLastmileProbeTest];
```

We also provide an open-source [OpenVideoCall-iOS](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-iOS) demo project on GitHub. You can try the demo, or view the source code in the [LastmileViewController.swift](https://github.com/AgoraIO/Basic-Video-Call/blob/master/Group-Video/OpenVideoCall-iOS/OpenVideoCall/LastmileViewController.swift) file.

### API Reference

- [`startLastmileProbeTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest)
- [`lastmileQuality`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:)
- [`lastmileProbeResult`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)

## Considerations
- You can conduct a last mile test only before joining a channel. Before the test ends, Agora recommends not calling any other API methods.
- The `lastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results.
- The audio SDK uses a fixed bitrate of 48 Kbps. The video SDK adjusts the actual bitrate according to the chosen video profile.





