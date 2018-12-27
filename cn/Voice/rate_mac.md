
---
title: 给通话评分
description: How to enable the rating function on macOS
platform: macOS
updatedAt: Thu Dec 27 2018 07:33:37 GMT+0000 (UTC)
---
# 给通话评分
## 功能描述

通话或直播结束后，让用户对通话/直播进行评分，可以收集用户对通话质量体验的主观评价，帮助改进产品。

Agora SDK 提供接口可以让你的用户为通话打分并提供反馈意见。

实现评分功能后，你可以在[水晶球](../../cn/Voice/aa_guide.md)的**通话调查**里看到用户对通话的评分，如下图所示：

![](https://web-cdn.agora.io/docs-files/1545801192291)

## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/mac_video.md)。

```swift
// swift
agoraKit.rate(agoraKit.getCallId(), 5, "This is an awesome call!");
```

```objective-c
// objective-c
NSString* callId = [agoraKit getCallId];
[agoraKit rate:callId rating:5 description:@"This is an awesome call!"]; 
```

### API 参考

- [`rate`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:)
- [`getCallId`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getCallId)

## 开发注意事项

- 所有相关的方法都有返回值。返回值小于 0 表示方法调用失败
