
---
title: 视频流回退
description: 
platform: Android
updatedAt: Fri Jul 17 2020 08:51:00 GMT+0800 (CST)
---
# 视频流回退
## 功能描述

网络不理想的环境下，音视频的质量都会下降。为提升用户体验，Agora 新增了 `setLocalPublishFallbackOption` 和 `setRemoteSubscribeFallbackOption` 两个方法。用户设置这两个方法后，在网络条件差、无法同时保证音频和视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或将媒体流回退为音频流，从而提高音视频质量。

## 实现方法

在实现视频流回退机制前，请确保已在你的项目中实现基本的音视频功能。详见[开始互动直播](../../cn/Video/start_live_android.md)或[开始音视频通话](../../cn/Video/start_call_android.md)。

参考如下步骤，在你的项目中实现视频流回退功能：

<div class="alert note">Agora 推荐你在加入频道前完成发送端和接收端的设置。</div>

### 发流端设置

1. 调用 `enableDualStreamMode` 方法开启[双流模式](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-duala双流模式)。

2. 调用 `setLocalPublishFallbackOption` 方法，并设置 `AUDIO_ONLY (2)` 开启本地发布的媒体流在弱网环境下回退。当网络不好时，SDK 会自动停止发布视频流，只发布音频流。

   本地发布的流从媒体流切换到音频流，或从音频流切换到媒体流时，SDK 会触发 `onLocalPublishFallbackToAudioOnly` 回调报告当前发布的流的类型。

   > CDN 推流直播场景下，发送端设置 `AUDIO_ONLY (2)` 可能导致 CDN 观众听到的声音有所延迟。Agora 不建议使用视频流回退机制。

### 接收端设置

1. 调用 `setRemoteSubscribeFallbackOption` 方法设置弱网环境下接收媒体流的回退选项。

   - 设置 `STREAM_LOW (1)`，则下行网络较弱时，只接收视频小流。
   - 设置 `AUDIO_ONLY (2)`，则下行网络较弱时，先尝试只接收视频小流。如果网络环境无法显示视频，则只接收音频流。

   用户接收到的流从媒体流切换到音频流，或从音频流切换到媒体流时，SDK 会触发 `onRemoteSubscribeFallbackToAudioOnly` 回调报告当前接收流的回退状态。当接收到的流从大流切换到小流，或从小流切换到大流时，SDK 会触发 `onRemoteVideoStats` 回调报告当前接收到的视频流类型。

### 示例代码

```java
//Java
// 开启视频双流模式。
rtcEngine.enableDualStreamMode(true);

 // 发送端的配置。网络较差时，只发送音频流。
rtcEngine.setLocalPublishFallbackOption(Constants.STREAM_FALLBACK_OPTION_AUDIO_ONLY);

 // 接收端的配置。弱网环境下先尝试接收小流；若当前网络环境无法显示视频，则只接收音频流。
rtcEngine.setRemoteSubscribeFallbackOption(Constants.STREAM_FALLBACK_OPTION_AUDIO_ONLY);
```

### API 参考

- [`enableDualStreamMode`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a645cb7d0f3a59dda27b157cf130c8c9a)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac8c08e79844a4e62e0670553484cbe90)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af64301ea1788dad0561aa678f3fe6ad3)
- [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a899d84de53c9e21de614a9611f6e062b)
- [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad14676019bf51b9d9a9c5520e6e578dd)
- [`onRemoteVideoStats`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#abb7af6e2827bbd03c6ab8338a0f616ca)



