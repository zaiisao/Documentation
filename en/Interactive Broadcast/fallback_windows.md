
---
title: Video Stream Fallback
description: 
platform: Windows
updatedAt: Mon Mar 02 2020 09:31:40 GMT+0800 (CST)
---
# Video Stream Fallback
## Introduction

The audio and video quality of a live broadcast or a video call deteriorates under poor network conditions. To improve the efficiency of a live broadcast, the `setLocalPublishFallbackOption` and `setRemoteSubscribeFallbackOption` methods are used for the SDK automatically switch the high-video stream to low-video stream and disbale the video stream under these conditions.


## Implementation

Before proceeding, ensure that you implement a basic live broadcast in your project. See [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_windows.md) for details.

Refer to the following steps to set the stream fallback under poor network conditions:

1. After joining the channel, the host calls the `enableDualStreamMode` method to enable [dual stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode).
2. The host calls the `setLocalPublishFallbackOption` method and set `AUDIO_ONLY (2)` to enable the fallback option for the locally published media stream based on the network conditions. SDK will disable the upstream video but enable audio only when the network conditions deteriorate and cannot support both video and audio streams.
	> Agora does not recommend using this method for CDN live streaming, because the remote CDN live user will have a noticeable lag when the locally published video stream falls back to audio only.
	
	When the local video stream falls back to audio only or when the audio only stream switches back to video, SDK triggers the `onLocalPublishFallbackToAudioOnly` callback to report current stream state to the local host.
3. Users in the channel call the `setRemoteSubscribeFallbackOption` method to set the subscribed stream fallback under poor network conditions.
	- Set `STREAM_LOW (1)` to only subscribe to the low-video stream from the host under poor network conditions.
	- Set `AUDIO_ONLY (2)` to susbcribe to the low-video stream or even audio stream from the host under poor network conditions.
	
	Once the remote media stream switches to the low stream due to poor network conditions, you can monitor the stream switch between a high and low stream in the `onRemoteVideoStats` callback. When the remotely subscribed video stream falls back to audio only or when the audio-only stream switches back to the video stream, the SDK triggers the `onRemoteSubscribeFallbackToAudioOnly` callback. 




### Sample code
```C++
// C++
// Enable the dual-stream mode (Configuration for the local).
int nRet = rep.enableDualStreamMode(static_cast<bool>(true));

// Configuration for the publisher. When the network condition is poor, send audio only. 
nRet = rep.setLocalPublishFallbackOption(static_cast<STREAM_FALLBACK_OPTIONS>(STREAM_FALLBACK_OPTION_AUDIO_ONLY));

// Configuration for the subscriber. Try to receive low stream under poor network conditions. When the current network conditions are not sufficient for video streams, receive audio stream only. 
nRet = rep.setRemoteSubscribeFallbackOption(static_cast<STREAM_FALLBACK_OPTIONS>(STREAM_FALLBACK_OPTION_AUDIO_ONLY));
```

### API reference

- [`enableDualStreamMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a72846f5bf13726e7a61497e2fef65972)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a6f411291eb8b834442b44361f78fa81f)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afd251e3f353a31d470ff9e60c3c3c5de)
- [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513)
- [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74)
- [`onRemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7163ffb650852be270ba0215b596d968)

