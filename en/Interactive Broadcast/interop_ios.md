
---
title: Enable Interoperability 
description: 
platform: iOS
updatedAt: Tue Dec 11 2018 20:03:11 GMT+0000 (UTC)
---
# Enable Interoperability 
## Introduction
To **enable interoperability** means that developers can enable real-time audio and video call services by integrating SDKs of different platfroms to their App, so that users of the App can join the same channel and interop with each other either from a mobile device (iOS  or Android), or from a desktop device (Windows or macOS), or even from a Web browser or Web App.

## Implementation

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md) for more information.

To enable interoperability between a mobile device and a Web browser or App, you need to do settings on both platforms. Notice that in the communication mode, interoperability is enabled by default and requires no further configuration.

* For the mobile device: call the `enableWebSdkInteroperability` API method.

	```swift
	// swift
	// ensure that this api is called from native side to interop with web sdk
	agoraKit.enableWebSdkInteroperability(true)
	```

	```objective-c
	// objective-c
	// ensure that this api is called from native side to interop with web  sdk
	[agoraKit enableWebSdkInteroperability: YES];
	```

*  For the Web: set the `mode` argument in the `createClient` API as `'live'`.

	```javascript
	// javascript
	// make sure corrent mode and codec is picked
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	```

## Considerations

* Configurations must be made on both the mobile device and the Web browser/App to enable interoperability.
* Use this function only when the channel is in the live broadcast mode. Interoperability is enabled by default in the communication mode.


