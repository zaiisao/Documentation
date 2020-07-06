
---
title: Video Stream Fallback
description: 
platform: Android
updatedAt: Mon Jul 06 2020 05:44:14 GMT+0800 (CST)
---
# Video Stream Fallback
## Introduction

The audio and video quality of live interactive video streaming or a video call deteriorates under poor network conditions. To improve the efficiency of live interactive video streaming or a video call, the `setLocalPublishFallbackOption` and `setRemoteSubscribeFallbackOption` methods are used for the SDK to automatically switch the high-video stream to low-video stream and disable the video stream under these conditions.


## Implementation

Before proceeding, ensure that you implement live interactive video streaming or a video call in your project. See [Start Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_android.md) or [Start a Call](../../en/Interactive%20Broadcast/start_call_android.md) for details.

Refer to the following steps to set the stream fallback under poor network conditions:

1. After joining the channel, the publisher calls the `enableDualStreamMode` method to enable [dual stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode).
2. The publisher calls the `setLocalPublishFallbackOption` method and set `AUDIO_ONLY (2)` to enable the fallback option for the locally published media stream based on the network conditions. The SDK will disable the upstream video and enable audio only when the network conditions deteriorate and cannot support both video and audio streams.
	> Agora does not recommend using this method for CDN live streaming, because the remote CDN live user will have a noticeable lag when the locally published video stream falls back to `audio only`.
	
	When the local video stream falls back to `audio only` or when the audio-only stream switches back to the video, the SDK triggers the `onLocalPublishFallbackToAudioOnly` callback to report current stream state to the local publisher.
3. The subscriber in the channel calls the `setRemoteSubscribeFallbackOption` method to set the subscribed stream fallback under poor network conditions.
	- Sets `STREAM_LOW (1)` to only subscribe to the low-video stream sent from the publisher under poor network conditions.
	- Sets `AUDIO_ONLY (2)` to subscribe to the low-video stream sent or even audio stream from the publisher under poor network conditions.
	
	Once the remote media stream is switched to the low stream due to poor network conditions, you can monitor the stream change between a high and low stream in the `onRemoteVideoStats` callback. When the remotely subscribed video stream falls back to `audio only` or when the audio-only stream switches back to the video stream, the SDK triggers the `onRemoteSubscribeFallbackToAudioOnly` callback. 


### Sample code

```java
//Java
// Enable the dual-stream mode (Configuration for the local).
rtcEngine.enableDualStreamMode(true);

// Configuration for the publisher. When the network condition is poor, send audio only. 
rtcEngine.setLocalPublishFallbackOption(Constants.STREAM_FALLBACK_OPTION_AUDIO_ONLY);

 // Configuration for the subscriber. Try to receive low stream under poor network conditions. When the current network conditions are not sufficient for video streams, receive audio stream only.    
 rtcEngine.setRemoteSubscribeFallbackOption(Constants.STREAM_FALLBACK_OPTION_AUDIO_ONLY);
```

### API reference

- [`enableDualStreamMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a645cb7d0f3a59dda27b157cf130c8c9a)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac8c08e79844a4e62e0670553484cbe90)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af64301ea1788dad0561aa678f3fe6ad3)
- [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a899d84de53c9e21de614a9611f6e062b)
- [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad14676019bf51b9d9a9c5520e6e578dd)
- [`onRemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#abb7af6e2827bbd03c6ab8338a0f616ca)


