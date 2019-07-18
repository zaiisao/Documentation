
---
title: Conduct a Last-mile Test
description: 
platform: Windows
updatedAt: Thu Jul 18 2019 03:38:35 GMT+0800 (CST)
---
# Conduct a Last-mile Test
## Introduction

To check if the uplink network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before joining the channel.

This function particularly applies to scenarios which have high requirements on the network quality.

> The audio SDK uses a fixed bitrate of 48 Kbps. The video SDK adjusts the actual bitrate according to the chosen video profile.



## Implementation 

```C++
// Implement the onLastmileQuality callback. 
void onLastmileQuality(int quality) {
 		// quality means the detected network quality type. You can use it for the related logic. 
		// ⑴ You can choose to end the last mile test in the callback. 
}

// Enable the last mile test at an appropriate time and before joining a channel. 
int ret = lpAgoraEngine->enableLastmileTest();

	// ⑵ You can also choose to end the last mile test at any time. Before the test ends, the onLastmileQuality callback can be returned multiple times. 
int ret = lpAgoraEngine->disableLastmileTest();

```

### API Reference
* [`enableLastmileTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2803623f129eeb92503a7a4e5a09a46d)
* [`disableLastmileTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a544fb9fda664578b80bbd7dbfffafd53)
* [`onLastmileQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33)

## Considerations

- You can conduct a last mile test only before joining a channel. Before the test ends, Agora recommends not calling any other API methods.
- The `onLastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 


