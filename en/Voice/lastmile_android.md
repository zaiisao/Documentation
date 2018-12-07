
---
title: Conduct a Last Mile Test
description: 
platform: Android
updatedAt: Fri Dec 07 2018 15:48:41 GMT+0000 (UTC)
---
# Conduct a Last Mile Test
## Introduction

The pre-call networkd quality test checks if the current network quality suits the audio bitrate or the target bitrate of the chosen video profile before a user joins a channel. Returned to the user by callbacks every two seconds, the test results are a network quality rating based on packet-loss rate and network jitter and reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 kbps; the video SDK adjust the actual bitrate according to the chosen video profile.



## Implementation 

Consumption: You have initialized rtcEngine before running the following sample code.

```Java
	// Call the enableLastmileTest function before joining a channel. 
	rtcEngine.enableLastmileTest();

	// The onLastmileQuality callback is in the global IRtcEngineEventHandler:
	public void onLastmileQuality(int quality) {
 		// Quality here refers to the detected network quality type. You can use it for the related logics. 
		// ⑴ You can choose to end the last mile test in the callback. 
		rtcEngine.disableLastmileTest();
	}

	// ⑵ You can also choose to end the lastmile test at any other time. Before the test ends, the onLastmileQuality() callback can be returned multiple times. 
	rtcEngine.disableLastmileTest();
```

## API Reference

- [RtcEngine.enableLastmileTest()](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35d045b585649ca89377ed82e9cf0662)
- [RtcEngine.disableLastmileTest()](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35d045b585649ca89377ed82e9cf0662)
- [IRtcEngineEventHandler.onLastmileQuality()](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5)

## Considerations

- Only before joining a channel can you conduct a last mile test.
- Chances are, the callback returns UNKNOWN the first time it is triggered. Still you can get the test result from the callbacks coming after it. 
