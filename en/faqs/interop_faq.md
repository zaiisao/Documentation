
---
title: How do I use the Agora Web SDK to interoperate with the Agora Native SDK?
description: 
platform: Android,iOS,macOS,Web,Windows
updatedAt: Tue Dec 31 2019 16:57:57 GMT+0800 (CST)
---
# How do I use the Agora Web SDK to interoperate with the Agora Native SDK?
In the communication profile, interoperability is enabled by default.

In the live broadcast profile, to enable interoperability between a mobile device and a web browser or app, you need to adjust the settings on both platforms. 

* On the mobile/desktop device, call the `enableWebSdkInteroperability` method.

	```java
	// java
	// Ensure that this methid is called from the native side to interoperate with Web SDK.
	rtcEngine.enableWebSdkInteroperability(true);
	```

	```swift
	// swift
	// Ensure that this method is called from the native side to interoperate with the Web SDK.
	agoraKit.enableWebSdkInteroperability(true)
	```

	```objective-c
	// objective-c
	// Ensure that this method is called from the native side to interoperate with the Web SDK.
	[agoraKit enableWebSdkInteroperability: YES];
	```

	```cpp
	// cpp
	//  Ensure that this method is called from the native side to interoperate with the Web SDK.
	lpAgoraEngine->enableWebSdkInteroperability
	```

* For the Web, in the `createClient` method, set the `mode` argument as `'live'`.

	```javascript
	// javascript
	// Choose the current mode and codec.
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	```

<div class="alert note">Given known test experience, if your scenario involves Safari, we recommend setting <code>codec</code> at the Web Client as <code>h264</code>; otherwise, set it as <code>vp8</code>.</div>





