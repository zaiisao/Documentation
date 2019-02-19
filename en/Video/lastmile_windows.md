
---
title: Conduct a Last Mile Test
description: 
platform: Windows
updatedAt: Thu Dec 27 2018 02:58:50 GMT+0000 (UTC)
---
# Conduct a Last Mile Test
## Introduction

You can conduct a last mile network quality test before starting a call to check if the network supports the audio bitrate or target bitrate of the chosen video profile before a user joins a channel. The `onLastmileQuality` callback reports the test results once every two seconds. The test results are based on the network quality ratings determined by the packet-loss rates and network jitter, which reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 Kbps. 
> The video SDK adjusts the actual bitrate according to the chosen video profile.



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

- You can conduct a last mile test only before joining a channel.
- The `onLastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 


