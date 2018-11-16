
---
title: 改善弱网环境下的用户体验
description: 
platform: iOS,macOS
updatedAt: Fri Nov 16 2018 08:57:45 GMT+0000 (UTC)
---
# 改善弱网环境下的用户体验
## 功能描述

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 `setLocalPublishFallbackOption` 和 `setRemoteSubscribeFallbackOption` 两个接口。 用户设置这两个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或直接关闭视频流，从而保证或提高音频质量。同时 SDK 会持续监控网络质量， 并在网络质量改善时恢复音视频流。在推流回退为音频流时，或由音频流恢复为音视频流，触发 `onLocalPublishFallbackToAudioOnly` 或 `onRemoteSubscribeFallbackToAudioOnly` 回调。

弱网环境下，SDK可以分别为发送和接收方进行如下两种策略的优化：

* 大小视频流切换，保证基本的视频体验。小流有着与大流相同的宽高比，但是分辨率和码率相对较低
* 极端情况下，可以选择只发送视频流，从而保证音频流的发送和接收

## 实现方式

```swift
// sets the local publish stream fallback option
agoraKit.setLocalPublishFallbackOption(.audioOnly)
// sets the remote subscribe stream fallback option
agoraKit.setRemoteSubscribeFallbackOption(.audioOnly)
```

```oc
[agoraKit setLocalPublishFallbackOption:AgoraStreamFallbackOptionAudioOnly];

[agoraKit setRemoteSubscribeFallbackOption:AgoraStreamFallbackOptionAudioOnly];
```
