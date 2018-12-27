
---
title: 给通话评分
description: How to enable the rating function on Windows
platform: Windows
updatedAt: Thu Dec 27 2018 07:33:42 GMT+0000 (UTC)
---
# 给通话评分
## 功能描述

通话或直播结束后，让用户对通话/直播进行评分，可以收集用户对通话质量体验的主观评价，帮助改进产品。

Agora SDK 提供接口可以让你的用户为通话打分并提供反馈意见。

实现评分功能后，你可以在[水晶球](../../cn/Voice/aa_guide.md)的**通话调查**里看到用户对通话的评分，如下图所示：

![](https://web-cdn.agora.io/docs-files/1545801192291)

## 实现方法

开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/windows_video.md)。

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

### API 参考

- [`rate`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a748c30a6339ec9798daa0d1b21585411)
- [`getCallId`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#af67688d89526926718edb26938d65541)

## 开发注意事项

- 所有相关的方法都有返回值。返回值小于 0 表示方法调用失败
