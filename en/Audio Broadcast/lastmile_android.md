
---
title: Conduct a Last-mile Test
description: 
platform: Android
updatedAt: Tue Jul 16 2019 07:42:42 GMT+0800 (CST)
---
# Conduct a Last-mile Test
## Introduction

To check if the uplink network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before joining the channel.

This function particularly applies to scenarios which have high requirements on the network quality.

> The audio SDK uses a fixed bitrate of 48 Kbps. The video SDK adjusts the actual bitrate according to the chosen video profile.



## Implementation 

Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/android_video.md).

Call the `startLastmileProbeTest` method before joining a channel to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).

Once this method is enabled, the SDK triggers the following callbacks:

- `onLastmileQuality`: Triggered two seconds after the `startLastmileProbeTest` method is called. This callback rates the network conditions with a score and is more closely linked to the user experience.
- `onLastmileProbeResult`: Triggered 30 seconds after the `startLastmileProbeTest` method is called. This callback returns the real-time statistics of the network conditions and is more objective.

```java
// Configure a LastmileProbeConfig instance.
LastmileProbeConfig config = new LastmileProbeConfig(){};
// Probe the uplink network quality.
config.probeUplink =  true;
// Probe the downlink network quality.
config.probeDownlink = true;
// The expected uplink bitrate (Kbps). The value range is [100, 5000].
config.expectedUplinkBitrate = 1000;
// The expected downlink bitrate (Kbps). The value range is [100, 5000].
config.expectedDownlinkBitrate = 1000;
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

// (2) Stop the test. Agora recommends not calling any other API method before the test ends.
rtcEngine.stopLastmileProbeTest();
```

### API Reference

- [`startLastmileProbeTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a81c6541685b1c4437d9779a095a0f871)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae21243b8da8bda9ee5f3a00621cbf959)
- [`onLastmileQuality`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5)
- [`onLastmileProbeResult`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad74a9120325bfeccdec4af4611110281)

## Considerations

- You can conduct a last mile test only before joining a channel. Before the test ends, Agora recommends not calling any other API methods.
- The `onLastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 
- The audio SDK uses a fixed bitrate of 48 Kbps. The video SDK adjusts the actual bitrate according to the chosen video profile.
