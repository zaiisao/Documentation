
---
title: Rate Call
description: 
platform: iOS,macOS
updatedAt: Sun Sep 29 2019 09:37:52 GMT+0800 (CST)
---
# Rate Call
## Introduction

When a call or live interactive streaming ends, you can gather feedback on the quality of experience to improve your product by asking your users to rate the call or live interactive streaming.

The Agora SDK provides methods for you to collect your users' ratings and comments on the calls.

After the rating function is implemented, you can see your users' ratings in [Agora Analytics](../../en/Interactive%20Broadcast/aa_guide.md), as shown in the figure below:

![](https://web-cdn.agora.io/docs-files/1545801217929)

## Implementation 

Before proceeding, ensure that you implement a basic video call or live broadcast in your project. See [Start a Call](../../en/Interactive%20Broadcast/start_call_ios.md)/[Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_ios.md) for details.

To rate the call:

1. When you are in the channel, call the `getCallId` method to get current call ID. 
2. After leaving the channel, call the `rate` method to rate the call.

### Sample code

```swift
// Swift
// Get current call ID. Rate 5 and give description.
agoraKit.rate(agoraKit.getCallId(), 5, "This is an awesome call!");
```

```objective-c
// Objective-C
// Get current call ID. Rate 5 and give description.
NSString* callId = [agoraKit getCallId];
[agoraKit rate:callId rating:5 description:@"This is an awesome call!"]; 
```

### API reference

- [`getCallId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getCallId)
- [`rate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:)
