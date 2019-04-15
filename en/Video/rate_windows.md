
---
title: Rate the Call
description: How to enable the rating function on Windows
platform: Windows
updatedAt: Thu Dec 27 2018 07:34:24 GMT+0800 (CST)
---
# Rate the Call
## Introduction

When a call or live broadcast ends, you can get feedback on the quality of experience to improve your product by asking your users to rate the call or live broadcast.

The Agora SDK provides methods for you to collect your users' ratings and comments on the calls.

After the rating function is implemented, you can see your users' ratings in [Agora Analytics](../../en/Video/aa_guide.md), as shown in the figure below:

![](https://web-cdn.agora.io/docs-files/1545801217929)

## Implementation

Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Video/windows_video.md) for more information.

```c++
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
 lpAgoraEngine->rate(wdCallId, 5, "This is an awesome call!");
#else
 lpAgoraEngine->rate(strCallId, 5, "This is an awesome call!");
#endif
```

### API Reference

- [`rate`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a748c30a6339ec9798daa0d1b21585411)
- [`getCallId`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#af67688d89526926718edb26938d65541)

## Considerations

The API methods have return values. If the method fails, the return value is < 0.
