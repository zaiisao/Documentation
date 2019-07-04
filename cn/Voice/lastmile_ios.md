
---
title: 通话前检测网络质量
description: 通话前的网络质量检测
platform: iOS,macOS
updatedAt: Thu Jul 04 2019 10:51:57 GMT+0800 (CST)
---
# 通话前检测网络质量
## 功能描述

通话前的网络质量检测用于在加入频道前检查用户当前网络状况是否满足音频码率或者当前选定的 Video Profile 的目标码率。检测结果通过每两秒钟一个回调通知用户, 结果是一个通过丢包率和网络 jitter 计算出来的网络质量打分，主要反映用户的上行网络状况。

> 纯语音产品使用 48 kbps 的固定探测码率；视频产品会根据当前选定的 Video Profile 调整探测码率。

## 实现方法

```swift
//Swift:
// 在合适的时机启动lastmile测试, 启用网络测试的情景请参考方法文档
agoraKit.enableLastmileTest()

// 注册回调
func rtcEngine(_ engine: AgoraRtcEngineKit, lastmileQuality quality: AgoraNetworkQuality) {
        // quality即为当前检测到的quality类型，可以根据此参数执行相关逻辑。
        // ⑴ 可以选择在回调内部结束测试
}

// ⑵ 也可以选择其它时候结束测试, 结束测试之前可能会被调用多次
agoraKit.disableLastmileTest()
```

```oc
//Objective-C
// 在合适的时机启动lastmile测试, 启用网络测试的情景请参考方法文档
[agoraKit enableLastmileTest];

// 注册回调
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine lastmileQuality:(AgoraNetworkQuality)quality {
}

// ⑵ 也可以选择其它时候结束测试, 结束测试之前调可能会被调用多次
[agoraKit disableLastmileTest];
```

### API参考

- [`enableLastmileTest`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLastmileTest)
- [`disableLastmileTest`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableLastmileTest)
- [`rtcEngine:lastmileQuality:`](https://docs.agora.io/cn/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:)

## 开发注意事项

- last-mile 测试必须在加入通话频道之前。
- 第一次回调的结果有一定概率是 unknown。如果回调第一次返回时结果是 unknown, 可通过之后的几次回调获得结果。
