
---
title: Rate Call
description: 
platform: Windows
updatedAt: Sun Sep 29 2019 09:37:39 GMT+0800 (CST)
---
# Rate Call
## Introduction

When a call or live interactive streaming ends, you can gather feedback on the quality of experience to improve your product by asking your users to rate the call or live interactive streaming.

The Agora SDK provides methods for you to collect your users' ratings and comments on the calls.

After the rating function is implemented, you can see your users' ratings in [Agora Analytics](../../en/Interactive%20Broadcast/aa_guide.md), as shown in the figure below:

![](https://web-cdn.agora.io/docs-files/1545801217929)

## Implementation 

Before proceeding, ensure that you implement a basic video call or live broadcast in your project. See [Start a Call](../../en/Interactive%20Broadcast/start_call_windows.md)/[Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_windows.md) for details.

To rate a call:

1. When you are in the channel, call the `getCallId` method to get current call ID. 
2. After leaving the channel, call the `rate` method to rate the call.

### Sample code

```C++
// C++
// Get current call ID.
agora::util::AString callId;
CString strCallId
lpAgoraEngine->getCallId(callId);

#ifdef UNICODE
 ::MultiByteToWideChar(CP_UTF8, 0, callId->c_str(), -1, strCallId.GetBuffer(128), 128);
 strCallId.ReleaseBuffer();
#else
 strCallId= callId->c_str();
#endif

#ifdef UNICODE
 CHAR wdCallId[MAX_PATH];

 ::WideCharToMultiByte(CP_UTF8, 0, strCallId, -1, wdCallId, MAX_PATH, NULL, NULL);
// Rate 5 and give description.
 lpAgoraEngine->rate(wdCallId, 5, "This is an awesome call!");
#else
// Rate 1 and give description.
 lpAgoraEngine->rate(strCallId, 1, "I don't like this call!");
#endif
```

### API reference

- [`getCallId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#af67688d89526926718edb26938d65541)
-  [`rate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a748c30a6339ec9798daa0d1b21585411)
