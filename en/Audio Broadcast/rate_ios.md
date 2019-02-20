
---
title: Rate the Call
description: How to enable the rating function on iOS
platform: iOS
updatedAt: Thu Dec 27 2018 07:34:14 GMT+0000 (UTC)
---
# Rate the Call
## Introduction

When a call or live broadcast ends, you can get feedback on the quality of experience to improve your product by asking your users to rate the call or live broadcast.

The Agora SDK provides methods for you to collect your users' ratings and comments on the calls.

After the rating function is implemented, you can see your users' ratings in [Agora Analytics](../../en/Audio%20Broadcast/aa_guide.md), as shown in the figure below:

![](https://web-cdn.agora.io/docs-files/1545801217929)

## Implementation
Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/ios_video.md).

```swift
// swift
agoraKit.rate(agoraKit.getCallId(), 5, "This is an awesome call!");
```

```objective-c
// objective-c
NSString* callId = [agoraKit getCallId];
[agoraKit rate:callId rating:5 description:@"This is an awesome call!"]; 
```


### API Reference

- [`rate`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:)
- [`getCallId`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getCallId)

## Considerations

The API methods have return values. If the method fails, the return value is < 0.
