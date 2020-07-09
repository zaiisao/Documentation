
---
title: Lastmile Tests
description: 
platform: Android
updatedAt: Thu Jul 09 2020 02:24:09 GMT+0800 (CST)
---
# Lastmile Tests
## Introduction

To check if the uplink network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before joining the channel.

This function particularly applies to scenarios which have high requirements on the network quality.



## Implementation 

Before conducting the last-mile test, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Voice/start_call_android.md) or [Start Live Interactive Streaming](../../en/Voice/start_live_android.md).

1. Call the `startLastmileProbeTest` method before joining a channel to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).
2. Once this method is enabled, the SDK triggers the following callbacks:
- `onLastmileQuality`: Triggered two seconds after the `startLastmileProbeTest` method is called. This callback rates the network conditions with a score and is more closely linked to the user experience.
- `onLastmileProbeResult`: Triggered 30 seconds after the `startLastmileProbeTest` method is called. This callback returns the real-time statistics of the network conditions and is more objective.
3. Call the `stopLastmileProbeTest` method to stop the last-mile network probe test.

### API call sequence

Refer to the following diagram to implement the last-mile test in your project.

![](https://web-cdn.agora.io/docs-files/1569464757177)

### Sample code

Refer to the following code to implement the last-mile test in your project.

```java
// Configure a LastmileProbeConfig instance.
LastmileProbeConfig config = new LastmileProbeConfig(){};
// Probe the uplink network quality.
config.probeUplink =  true;
// Probe the downlink network quality.
config.probeDownlink = true;
// The expected uplink bitrate (bps). The value range is [100000, 5000000].
config.expectedUplinkBitrate = 100000;
// The expected downlink bitrate (bps). The value range is [100000, 5000000].
config.expectedDownlinkBitrate = 100000;
// Start the last-mile network test before joining the channel.
rtcEngine.startLastmileProbeConfig(config);

// Implemented in the global IRtcEngineEventHandler class.
// Triggered 2 seconds after starting the last-mile test.
public void onLastmileQuality(int quality)

// Implemented in the global IRtcEngineEventHandler class.
// Triggered 30 seconds after starting the last-mile test.
public void onLastmileProbeResult(LastmileProbeResult) {
	// (1) Stop the test. Agora recommends not calling any other API method before the test ends.
	rtcEngine.stopLastmileProbeTest();
}

// (2) Stop the test in an alternate place. Agora recommends not calling any other API method before the test ends.
rtcEngine.stopLastmileProbeTest();
```

We also provide an open-source [OpenVideoCall-Android](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-Android) demo project on GitHub. You can try the demo, or view the source code in the [NetworkTestActivity.java](https://github.com/AgoraIO/Basic-Video-Call/blob/master/Group-Video/OpenVideoCall-Android/app/src/main/java/io/agora/openvcall/ui/NetworkTestActivity.java) file.

### API Reference

- [`startLastmileProbeTest`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a81c6541685b1c4437d9779a095a0f871)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae21243b8da8bda9ee5f3a00621cbf959)
- [`onLastmileQuality`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5)
- [`onLastmileProbeResult`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad74a9120325bfeccdec4af4611110281)

## Considerations

- You can conduct a last mile test only before joining a channel. Before the test ends, Agora recommends not calling any other API methods.
- The `onLastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 
- The audio SDK uses a fixed bitrate of 48 Kbps. The video SDK adjusts the actual bitrate according to the chosen video profile.
