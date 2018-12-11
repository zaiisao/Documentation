
---
title: Test or Select a Media Device
description: 
platform: Android
updatedAt: Fri Nov 23 2018 09:50:01 GMT+0000 (UTC)
---
# Test or Select a Media Device
## Introduction

Agora supports the media device test and selection, allowing you to check if a camera or an audio device (a headeset, microphone, or speaker) functions well or connects properly to the SD-RTN. For example, in an audio device test, if a user's speech can be replayed in 10 seconds, then the system's audio device is properly connected to the SD-RTN.

You can use the media device test and selection feature in the following scenarios:

- As a host in a live broadcast, do a self-check before airing the broadcast.
- As an online user, check if the media device works well.

## Implementation

```Java
	// Start the echo test. 
	rtcEngine.startEchoTest();

	// Wait and check if you can hear the voice of yours.

	// Stop the echo test. 
	rtcEngine.stopEchoTest();
```

## API Reference

- [startEchoTest()](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac93b84c9ebbb32f5ee304732804ec1b9)
- [stopEchoTest()](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a01b8067275003c011f6d81bb41ee0fe1)

## Considerations

- After calling `startEchoTest`, you must call `stopEchoTest` to stop the test. Otherwise, you cannot proceed with a second test or call `joinChannel` to make a call. 
- In the Live Broadcast profile, only the host (BROADCASTER) can call the `startEchoTest` method. If you have swtiched your profile from Communication to Live Broadcast, ensure that you call the `setClientRole` method to switch your role to BROADCASTER before calling `startEchoTest`.
