
---
title: 视频流回退机制
description: 
platform: iOS,macOS
updatedAt: Wed Sep 25 2019 03:53:33 GMT+0800 (CST)
---
# 视频流回退机制
## 功能描述

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 `setLocalPublishFallbackOption` 和 `setRemoteSubscribeFallbackOption` 两个接口。 用户设置这两个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或直接关闭视频流，从而保证或提高音频质量。同时 SDK 会持续监控网络质量， 并在网络质量改善时恢复音视频流。在推流回退为音频流时，或由音频流恢复为音视频流，触发 `onLocalPublishFallbackToAudioOnly` 或 `onRemoteSubscribeFallbackToAudioOnly` 回调。

弱网环境下，SDK可以分别为发送和接收方进行如下两种策略的优化：

* 大小视频流切换，保证基本的视频体验。小流有着与大流相同的宽高比，但是分辨率和码率相对较低
* 极端情况下，可以选择只发送音频流，从而保证音频流的发送和接收

## 实现方式

```swift
// Swift
// 开启视频大小流模式
agoraKit.enableDualStreamMode(true);

// 发送方设置
// 网络较差时，只发送音频流
agoraKit.setLocalPublishFallbackOption(.audioOnly)

// 接收远端视频的配置
// 弱网环境下先尝试接收小流；若当前网络环境无法显示视频，则只接受音频
agoraKit.setRemoteSubscribeFallbackOption(.audioOnly)

// 若开发者希望主动控制远端视频的大小流并进行切换，则调用以下方式
// 将以 uid 识别的远端用户的视频切换成小流。
// 注意：必须设置成双流模式才能真正获取小流视频
agoraKit.setRemoteVideoStream(uid, type: .low)
```

```oc
//Objective-C
// 开启视频大小流模式
agoraKit.enableDualStreamMode(true);

// 发送方设置
// 网络较差时，只发送音频流
[agoraKit setLocalPublishFallbackOption: AgoraStreamFallbackOptionAudioOnly];

// 接收远端视频的配置
// 弱网环境下先尝试接收小流；若当前网络环境无法显示视频，则只接受音频
[agoraKit setRemoteSubscribeFallbackOption: AgoraStreamFallbackOptionAudioOnly];

// 若开发者希望主动控制远端视频的大小流并进行切换，则调用以下方式
// 将以 uid 识别的远端用户的视频切换成小流。
// 注意：必须设置成双流模式才能真正获取小流视频
[agoraKit setRemoteVideoStream: uid type:AgoraVideoStreamTypeLow];
```

### API 参考

- [`enableDualStreamMode:`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:)
- [`setRemoteVideoStream:type:`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVideoStream:type:)
- [`setLocalPublishFallbackOption:`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:)
- [`setRemoteSubscribeFallbackOption:`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:)

