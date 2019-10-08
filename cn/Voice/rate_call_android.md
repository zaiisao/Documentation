
---
title: 通话后评分
description: 
platform: Android
updatedAt: Sun Sep 29 2019 08:26:45 GMT+0800 (CST)
---
# 通话后评分
## 功能描述
通话或直播结束后，让用户对通话/直播进行评分，可以收集用户对通话质量体验的主观评价，帮助改进产品。

Agora SDK 提供接口可以让你的用户为通话打分并提供反馈意见。

实现评分功能后，你可以在[水晶球](../../cn/Voice/aa_guide.md)的**通话调查**里看到用户对通话的评分，如下图所示：

![](https://web-cdn.agora.io/docs-files/1545801192291)

## 实现方法

在实现给通话质量评分功能前，请确保你已在项目中完成基本的实时音视频功能。详见[开始音视频通话](../../cn/Voice/start_call_android.md)或[开始互动直播](../../cn/Voice/start_live_android.md)。

参考如下步骤，在你的项目中实现给通话质量打分：

1. 加入频道后，用户调用 `getCallId` 方法获取当前通话 ID。
2. 离开频道后，用户调用 `rate` 方法给该通话的质量进行评分。

### 示例代码

```java
// Java
// 获取通话 ID，给通话质量评 5 分，并进行描述。
String callId = rtcEngine.getCallId();
rtcEngine.rate(callId, 5, "This is an awesome call!");
```

### API 参考

- [`getCallId`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa4d80e8de0e8ae4d2fd3f153945d289f)
- [`rate`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab7083355af531cc43d455024bd1f7662)



