
---
title: Pre-call Network and Device Tests
description: 
platform: Windows
updatedAt: Tue Nov 03 2020 07:58:05 GMT+0800 (CST)
---
# Pre-call Network and Device Tests
## Introduction

To check if the uplink network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before joining the channel.

This function particularly applies to scenarios which have high requirements on the network quality.



## Implementation 

Before conducting the last-mile test, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_windows.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_windows.md).

1. Call the `startLastmileProbeTest` method before joining a channel to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).
2. Once this method is enabled, the SDK triggers the following callbacks:
- `onLastmileQuality`: Triggered two seconds after the `startLastmileProbeTest` method is called. This callback rates the network conditions with a score and is more closely linked to the user experience.
- `onLastmileProbeResult`: Triggered 30 seconds after the `startLastmileProbeTest` method is called. This callback returns the real-time statistics of the network conditions and is more objective.
3. Call the `stopLastmileProbeTest` method to stop the last-mile network probe test.

### API call sequence

Refer to the following diagram to implement the last-mile test in your project.

![](https://web-cdn.agora.io/docs-files/1569466455559)

### Sample code

Refer to the following code to implement the last-mile test in your project.

```cpp
// Register the callback events.
// Triggered 2 seconds after starting the last-mile test.
void onLastmileQuality(int quality) {
}

// Triggered 30 seconds after starting the last-mile test.
void onLastmileProbeResult(LastmileProbeResult) {
  // (1) Stop the test. Agora recommends not calling any other API method before the test ends.
  lpAgoraEngine->stopLastmileProbeTest();
}

// Configure a LastmileProbeConfig instance.
LastmileProbeConfig config;
// Probe the uplink network quality.
config.probeUplink = true;
// Probe the downlink network quality.
config.probeDownlink = true;
// The expected uplink bitrate (bps). The value range is [100000, 5000000].
config.expectedUplinkBitrate = 100000;
// The expected downlink bitrate (bps). The value range is [100000, 5000000].
config.expectedDownlinkBitrate = 100000;
// Start the last-mile network test before joining the channel.
lpAgoraEngine->startLastmileProbeTest(config);

// (2) Stop the test in an alternate place. Agora recommends not calling any other API method before the test ends.
lpAgoraEngine->stopLastmileProbeTest();
```


### API Reference

* [`startLastmileProbeTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adb3ab7a20afca02f5a5ab6fafe026f2b)
* [`stopLastmileProbeTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a94f3494035429684a750e1dee7ef1593)
* [`onLastmileQuality`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33)
* [`onLastmileProbeResult`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a44134dfda5d412831fa8e44fa533fca5)

## Considerations

- You can conduct a last mile test only before joining a channel. Before the test ends, Agora recommends not calling any other API methods.
- The `onLastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 
- The audio SDK uses a fixed bitrate of 48 Kbps. The video SDK adjusts the actual bitrate according to the chosen video profile.


