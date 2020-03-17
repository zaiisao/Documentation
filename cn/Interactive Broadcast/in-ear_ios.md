
---
title: 耳返
description: How to enable ear-monitoring and adjust the volume of ear-monitoring.
platform: iOS
updatedAt: Wed Sep 25 2019 10:09:18 GMT+0800 (CST)
---
# 耳返
## 功能描述
耳返主要实现监听的功能，在低延时的情况下可以给主播一个比较真实的反馈，在演唱会等专业场景里比较常用。
Agora SDK 支持耳返功能，同时支持调节耳返的音量。

## 实现方法
在实现耳返功能前，请确保已在你的项目中实现基本的实时音视频功能。详见[实现音视频通话](../../cn/Interactive%20Broadcast/start_call_ios.md)或[实现互动直播](../../cn/Interactive%20Broadcast/start_live_ios.md)。

Agora SDK 提供 `enableInEarMonitoring` 和 `setInEarMonitoringVolume` 方法给开发者根据场景需求灵活配置耳返功能，该方法在 `joinChannelByToken` 前后均可调用。

### 示例代码

```swift
// swift
// 设置开启耳返监听功能，默认为 false
agoraKit.enable(inEarMonitoring: true)

// 设置耳返的音量，volume 的取值范围为 0 ~ 100，默认为 100，代表麦克风录到的原始音量
agoraKit.setInEarMonitoringVolume(50)
```

```objective-c
//objective-c
// 设置开启耳返监听功能，默认为 NO
[agoraKit enableInEarMonitoring:YES];

// 设置耳返的音量，volume的取值范围为 0 ~ 100，默认为 100，代表麦克风录到的原始音量
[agoraKit setInEarMonitoringVolume: 50];
```

### API 参考

- [`enableInEarMonitoring`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableInEarMonitoring:)
- [`setInEarMonitoringVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setInEarMonitoringVolume:)

## 开发注意事项

- 在 `enableInEarMonitoring` 后调用 `setInEarMonitoringVolume`。
- 以上方法都有返回值，返回值小于 0 表示方法调用失败。

