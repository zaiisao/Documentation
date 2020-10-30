
---
title: 通话前检测网络质量
description: 通话前的网络质量检测
platform: macOS
updatedAt: Mon Feb 24 2020 13:52:02 GMT+0800 (CST)
---
# 通话前检测网络质量
## 功能描述

在加入频道或切换角色为主播前，进行网络质量探测，可以判断或预测用户当前的网络状况是否良好，可以满足音频码率或者当前选定的视频属性的目标码率。

在对网络质量要求高的场景下，Agora 建议在加入频道前进行探测，保证通信顺畅。

## 实现方法

开始检测网络质量前，请确保你已在项目中实现了基本的音视频通信或直播功能。详见如下文档：
- iOS：[开始音视频通话](../../cn/Interactive%20Broadcast/start_call_ios.md)或[开始互动直播](../../cn/Interactive%20Broadcast/start_live_ios.md)
- macOS：[开始音视频通话](../../cn/Interactive%20Broadcast/start_call_mac.md)或[开始互动直播](../../cn/Interactive%20Broadcast/start_live_mac.md)

1. 在用户加入频道或上麦前，调用 `startLastmileProbeTest` 进行网络质量探测，向用户反馈上下行网络的带宽、丢包、网络抖动和往返时延。
2. 启用该方法后，SDK 会依次返回如下 2 个回调：
- `lastmileQuality`：约 2 秒内返回。该回调通过打分反馈上下行网络质量，更贴近主观感受
- `lastmileProbeResult`：约 30 秒内返回。该回调通过客观数据反馈上下行网络质量，更客观
3. 获取到网络质量数据后，调用 `stopLastmileProbeTest` 停止通话前网络质量探测。

### API 时序图

参考下图时序在你的项目中实现通话前网络探测功能。

![](https://web-cdn.agora.io/docs-files/1569465210614)

### 示例代码

参考下文代码在你的项目中实现通话前网络探测功能。

```swift
// Swift
// 配置一个 LastmileProbeConfig 实例。参数可参考 API 文档
let config = AgoraLastmileProbeConfig(
  // 确认探测上行网络质量
  probeUplink: true, 
  // 确认探测下行网络质量
  probeDownlink: true,
  // 期望的最高发送码率，单位为 bps，范围为 [100000, 5000000]
  expectedUplinkBitrate: 100000, 
  // 期望的最高接收码率，单位为 bps，范围为 [100000, 5000000]
  expectedDownlinkBitrate: 100000)
// 加入频道前开始 Last-mile 网络探测
agoraKit.startLastmileProbeTest(config)

// 注册回调
// 开始 Last-mile 网络探测后，约 2 秒后会发生该回调
// quality 即为当前检测到的 quality 类型，可以根据此参数执行相关逻辑
func rtcEngine(_ engine: AgoraRtcEngineKit, lastmileQuality quality: AgoraNetworkQuality) {
}

// 开始 Last-mile 网络探测后，约 30 秒后会发生该回调
// result 即为当前探测到的网络质量结果，可以根据此类执行相关逻辑
func rtcEngien(_ engine: AgoraRtcEngineKit, lastmileProbeResult result: AgoraLastmileProbeResult){
  // (1)可以选择在回调内部结束测试。在测试结束前，Agora 建议不要调用其他 API 方法
  agoraKit.stopLastmileProbeTest()  
}

// (2)也可以选择在其他时候结束测试。在测试结束前，Agora 建议不要调用其他 API 方法
agoraKit.stopLastmileProbeTest()
```

```objective-c
// Objective-C
// 配置一个 LastmileProbeConfig 实例。参数可参考 API 文档
AgoraLastmileProbeConfig *config = [[AgoraLastmileProbeConfig alloc] probeUplink: YES probeDownlink: YES expectedUplinkBitrate: 100000 expectedDownlinkBitrate: 100000];
// 加入频道前开始 Last-mile 网络探测
[agoraKit startLastmileProbeTest: config];

// 注册回调
// 开始 Last-mile 网络探测后，约 2 秒后会发生该回调
// quality 即为当前检测到的 quality 类型，可以根据此参数执行相关逻辑
- (void)rtcEngine:(AgoraRtcEngine * _Nonnull)engine lastmileQuality: (AgoraNetworkQuality)quality {
}

// 开始 Last-mile 网络探测后，约 30 秒后会发生该回调
// result 即为当前探测到的网络质量结果，可以根据此类执行相关逻辑
- (void)rtcEngine:(AgoraRtcEngine * _Nonnull)engine lastmileProbeResult: (AgoraLastmileProbeResult)result {
  // (1)可以选择在回调内部结束测试。在测试结束前，Agora 建议不要调用其他 API 方法
  [agoraKit stopLastmileProbeTest];
}

// (2)也可以选择在其他时候结束测试。在测试结束前，Agora 建议不要调用其他 API 方法
[agoraKit stopLastmileProbeTest];
```

同时，我们在 GitHub 提供一个开源的 [OpenVideoCall-iOS](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-iOS) 示例项目。你可以下载体验，或者查看 [LastmileViewController.swift](https://github.com/AgoraIO/Basic-Video-Call/blob/master/Group-Video/OpenVideoCall-iOS/OpenVideoCall/LastmileViewController.swift) 文件中的源代码。

### API参考

- [`startLastmileProbeTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)
- [`stopLastmileProbeTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest)
- [`lastmileQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:)
- [`lastmileProbeResult`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)

## 开发注意事项

- Last-mile 测试必须在加入通话频道之前。在结束测试之前，Agora 不建议调用其他 API 方法。
- `lastmileQuality` 回调第一次报告的结果有一定概率是 unknown, 可通过之后的几次回调获得结果。
- 纯语音产品使用 48 Kbps 的固定探测码率；视频产品会根据当前选定的视频属性调整探测码率。
