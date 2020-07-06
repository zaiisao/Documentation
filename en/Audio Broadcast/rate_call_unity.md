
---
title: Rate Call
description: 
platform: Unity
updatedAt: Mon Jul 06 2020 06:09:10 GMT+0800 (CST)
---
# Rate Call
## Introduction

When a call or live interactive streaming ends, you can gather feedback on the quality of experience to improve your product by asking your users to rate the call or live interactive streaming.

The Agora SDK provides methods for you to collect your users' ratings and comments on the calls.

After the rating function is implemented, you can see your users' ratings in [Agora Analytics](../../en/Audio%20Broadcast/aa_guide.md), as shown in the figure below:

![](https://web-cdn.agora.io/docs-files/1545801217929)

## Implementation 

Before proceeding, ensure that you implement a basic video call or live interactive streaming in your project. See [Start a Call](../../en/Audio%20Broadcast/start_call_unity.md)/[Start Live Interactive Video Streaming](../../en/Audio%20Broadcast/start_live_unity.md) for details.

To rate a call:

1. When you are in the channel, call the `GetCallId` method to get current call ID. 
2. After leaving the channel, call the `Rate` method to rate the call.

### Sample code

```C#
// Get current call ID. Rate 5 and give description.
String callId = mRtcEngine.GetCallId();
mRtcEngine.Rate(callId, 5, "This is an awesome call!");
```

### API reference

- [`GetCallId`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ab6b0ec1b64c5c9ec417819af0c70385a)
- [`Rate`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a2de30387e035e21f20f5bf5aebc001f5)
