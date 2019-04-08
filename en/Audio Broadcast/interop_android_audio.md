
---
title: Enable Interoperability 
description: 
platform: Android
updatedAt: Mon Apr 08 2019 09:36:12 GMT+0000 (UTC)
---
# Enable Interoperability 
## Introduction
To **enable interoperability** means enabling real-time audio and video call services by integrating SDKs of different platforms in the app. Users of the app can then join the same channel and interoperate with others using a mobile device (iOS or Android), a desktop device (Windows or macOS), or a web browser or web app.

## Implementation
Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/android_video.md).

To enable interoperability between a mobile device and a web browser or app, you need to adjust the settings on both platforms. 

> Use this function only when the channel is in the live broadcast profile. Interoperability is enabled by default in the communication profile.

* On the mobile device, call the `enableWebSdkInteroperability` method.

	```java
	// java
	// Ensure that this method is called from the native side to interoperate with the Web SDK.
	rtcEngine.enableWebSdkInteroperability(true);
	```

* For the Web, in the `createClient` method, set the `mode` argument as `'live'`.

	```javascript
	// javascript
	// Choose the current mode and codec.
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	```

## Considerations
* The settings must be adjusted on both the mobile device and the web browser/app to enable interoperability.
* Use this function only when the channel is in the live broadcast profile. Interoperability is enabled by default in the communication profile.
* Given known test experience, if your scenario involves Safari, we recommend setting codec at the Web Client as h264; otherwise, set it as vp8.

	
