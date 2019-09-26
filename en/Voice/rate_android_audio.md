
---
title: Rate the Call
description: How to enable the rating function on Android
platform: Android
updatedAt: Thu Dec 27 2018 07:34:37 GMT+0800 (CST)
---
# Rate the Call
## Introduction

When a call or live broadcast ends, you can gather feedback on the quality of experience to improve your product by asking your users to rate the call or live broadcast.

The Agora SDK provides methods for you to collect your users' ratings and comments on the calls.

After the rating function is implemented, you can see your users' ratings in [Agora Analytics](../../en/Voice/aa_guide.md), as shown in the figure below:

![](https://web-cdn.agora.io/docs-files/1545801217929)

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Voice/android_audio.md).

```java
String callId = rtcEngine.getCallId();
rtcEngine.rate(callId, 5, "This is an awesome call!");
```

### API Reference

- [`rate`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab7083355af531cc43d455024bd1f7662)
- [`getCallId`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa4d80e8de0e8ae4d2fd3f153945d289f)

## Considerations

The API methods have return values. If the method call fails, the return value is < 0.
