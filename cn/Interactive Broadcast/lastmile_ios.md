
---
title: 通话前的网络质量检测
description: 通话前的网络质量检测
platform: macOS,iOS
updatedAt: Fri Nov 16 2018 06:17:45 GMT+0000 (UTC)
---
# 通话前的网络质量检测
## 功能描述

通话前的网络质量检测用于在加入频道前检查用户当前网络状况是否满足当前选定的 video profile 的目标码率。检测结果通过每两秒钟一个回调通知用户, 结果是一个通过丢包率和网络 jitter 计算出来的网络质量打分，主要反映用户的上行网络状况。

## 实现方法

```swift
// tests the quality of the user’s network connection and is disabled by default
agoraKit.enableLastmileTest()
// disables the network connection quality test
agoraKit.disableLastmileTest()

// register callback
func rtcEngine(_ engine: AgoraRtcEngineKit, lastmileQuality quality: AgoraNetworkQuality) {
        
}
```

```oc
[agoraKit disableLastmileTest];

- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine lastmileQuality:(AgoraNetworkQuality)quality {
}
```

## 开发注意事项

- 远端视频/音频流信息获取仅在加入频道后且订阅对应流的状态下有效
