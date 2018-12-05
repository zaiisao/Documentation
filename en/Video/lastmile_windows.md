
---
title: Conduct a Last Mile Test
description: 
platform: Windows
updatedAt: Wed Dec 05 2018 03:28:21 GMT+0000 (UTC)
---
# Conduct a Last Mile Test
## Introduction

The pre-call networkd quality test checks if the current network quality suits the audio bitrate or the target bitrate of the chosen video profile before a user joins a channel. Returned to the user by callbacks every two seconds, the test results are a network quality rating based on packet-loss rate and network jitter and reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 kbps; the video SDK adjust the actual bitrate against the chosen video profile.



## Implementation 

```C++
// Implement the onLastmileQuality callback. 
void onLastmileQuality(int quality) {
 		// Quality here refers to the detected network quality type. You can use it for the related logics. 
		// ⑴ You can choose to end the last mile test in the callback. 
}

// Enable the last mile test at an appropriate time and before joining a channel. 
int ret = lpAgoraEngine->enableLastmileTest();

	// ⑵ You can also choose to end the last mile test at any other time. Before the test ends, the onLastmileQuality() callback can be returned multiple times. 
int ret = lpAgoraEngine->disableLastmileTest();

```

## API Reference
* [enableLastmileTest](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2803623f129eeb92503a7a4e5a09a46d)
* [disableLastmileTest](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a544fb9fda664578b80bbd7dbfffafd53)
* [onLastmileQuality](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33)

## Considerations

- Only before joining a channel can you conduct a lastmile test.
- Chances are, the callback returns UNKNOWN the first time it is triggered. Still you can get the test result from the callbacks coming after it. 


