
---
title: Test or Select a Media Device
description: 
platform: Android
updatedAt: Wed Apr 10 2019 07:02:44 GMT+0000 (UTC)
---
# Test or Select a Media Device
## Introduction

Agora supports the media device test and selection feature, allowing you to check if a camera or an audio device (a headset, microphone, or speaker) works or connects properly to the SD-RTN. For example, in an audio device test, if a user's recorded voice is replayed in 10 seconds, then the system's audio device works and is connected properly to the SD-RTN.

You can use the media device test and selection feature in the following scenarios:

- A host self-checks before starting a live broadcast.
- An online user checks if the media device works.

## Implementation

```Java
	// Start the echo test. 
	rtcEngine.startEchoTest();

	// Wait and check if you can hear your voice.

	// Stop the echo test. 
	rtcEngine.stopEchoTest();
```

### API Reference

- [`startEchoTest()`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac93b84c9ebbb32f5ee304732804ec1b9)
- [`stopEchoTest()`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a01b8067275003c011f6d81bb41ee0fe1)

## Considerations

- After calling the `startEchoTest` method to start the test, you must call the `stopEchoTest` method to stop the test. Otherwise, you cannot proceed with a second test or call the `joinChannel` method to make a call. 
- In the Live Broadcast profile, only the host (BROADCASTER) can call the `startEchoTest` method. If you swtiched your profile from Communication to Live Broadcast, ensure that you call the `setClientRole` method to switch your role to BROADCASTER before calling the `startEchoTest` method.
