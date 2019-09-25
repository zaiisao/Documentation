
---
title: 视频流回退机制
description: 
platform: Windows
updatedAt: Wed Sep 25 2019 08:17:22 GMT+0800 (CST)
---
# 视频流回退机制
## 功能描述

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 `setLocalPublishFallbackOption` 和 `setRemoteSubscribeFallbackOption` 两个接口。 用户设置这两个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或直接关闭视频流，从而保证或提高音频质量。同时 SDK 会持续监控网络质量， 并在网络质量改善时恢复音视频流。在推流回退为音频流时，或由音频流恢复为音视频流，触发 `onLocalPublishFallbackToAudioOnly` 或 `onRemoteSubscribeFallbackToAudioOnly` 回调。

弱网环境下，SDK可以分别为发送和接收方进行如下两种策略的优化：

* 大小视频流切换，保证基本的视频体验。小流有着与大流相同的宽高比，但是分辨率和码率相对较低
* 极端情况下，可以选择只发送音频流，从而保证音频流的发送和接收

## 实现方法

```C++
// 初始化参数对象
RtcEngineParameters rep(*lpAgoraEngine);

// 开启视频双流模式(近端设置)
int nRet = rep.enableDualStreamMode(static_cast<bool>(true));

// 网络较差时，只发送音频流(近端设置)
nRet = rep.setLocalPublishFallbackOption(static_cast<STREAM_FALLBACK_OPTIONS>(STREAM_FALLBACK_OPTION_AUDIO_ONLY));

// 弱网环境下先尝试接收小流；若当前网络环境无法显示视频，则只接受音频(远端设置)
nRet = rep.setRemoteSubscribeFallbackOption(static_cast<STREAM_FALLBACK_OPTIONS>(STREAM_FALLBACK_OPTION_AUDIO_ONLY));

```

### API 参考
* [`enableDualStreamMode`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a65faf883ce4aa9d596741552825cbd33)
* [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec)
* [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7)

### 注意事项
* 在网络不是很好的时候，这个引擎会自动调整，需要对应提示用户预期
