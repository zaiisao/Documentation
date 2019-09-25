
---
title: Conduct a Last-mile Test
description: Conduct a Pre-call Lastmile Test
platform: iOS,macOS
updatedAt: Thu Jul 18 2019 03:04:46 GMT+0800 (CST)
---
# Conduct a Last-mile Test
## Introduction

To check if the uplink network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before joining the channel.

This function particularly applies to scenarios which have high requirements on the network quality.



## Implementation

Ensure that you prepare the development. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md).

Call the `startLastmileProbeTest` method before joining a channel to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).

Once this method is enabled, the SDK triggers the following callbacks:

- `lastmileQuality`: Triggered two seconds after the `startLastmileProbeTest` method is called. This callback rates the network conditions with a score and is more closely linked to the user experience.
- `lastmileProbeResult`: Triggered 30 seconds after the `startLastmileProbeTest` method is called. This callback returns the real-time statistics of the network conditions and is more objective.

```swift
// swift
//  Configure a LastmileProbeConfig instance.
let config = AgoraLastmileProbeConfig(
  //Probe the uplink network quality.
  probeUplink: true, 
  // Probe the downlink network quality.
  probeDownlink: true,
  // The expected uplink bitrate (Kbps). The value range is [100, 5000].
  expectedUplinkBitrate: 1000, 
  // The expected downlink bitrate (Kbps). The value range is [100, 5000].
  expectedDownlinkBitrate: 1000)
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

```oc
// objective-c
//  Configure a LastmileProbeConfig instance.
AgoraLastmileProbeConfig *config = [[AgoraLastmileProbeConfig alloc] probeUplink: YES probeDownlink: YES expectedUplinkBitrate: 1000 expectedDownlinkBitrate: 1000];
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

// (2) Stop the test. Agora recommends not calling any other API method before the test ends.
[agoraKit stopLastmileProbeTest];
```

### API Reference

- [`startLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest)
- [`lastmileQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:)
- [`lastmileProbeResult`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)

## Considerations

- You can conduct a last mile test only before joining a channel. Before the test ends, Agora recommends not calling any other API methods.
- The `lastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 




