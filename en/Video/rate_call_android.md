
---
title: Rate Call
description: 
platform: Android
updatedAt: Sun Sep 29 2019 09:38:06 GMT+0800 (CST)
---
# Rate Call
## Introduction

When a call or live broadcast ends, you can gather feedback on the quality of experience to improve your product by asking your users to rate the call or live broadcast.

The Agora SDK provides methods for you to collect your users' ratings and comments on the calls.

After the rating function is implemented, you can see your users' ratings in [Agora Analytics](../../en/Video/aa_guide.md), as shown in the figure below:

![](https://web-cdn.agora.io/docs-files/1545801217929)

## Implementation 

Before proceeding, ensure that you implement a basic video call or live broadcast in your project. See [Start a Call](../../en/Video/start_call_android.md)/[Start a Live Broadcast](../../en/Video/start_live_android.md) for details.

To rate a call:

1. When you are in the channel, call the `getCallId` method to get current call ID. 
2. After leaving the channel, call the `rate` method to rate the call.

### Sample code

```java
// Java
// Get current call ID. Rate 5 and give description.
String callId = rtcEngine.getCallId();
rtcEngine.rate(callId, 5, "This is an awesome call!");
```

### API reference

- [`getCallId`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa4d80e8de0e8ae4d2fd3f153945d289f)
- [`rate`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab7083355af531cc43d455024bd1f7662)
