
---
title: Improve Experience Under Poor Network Conditions
description: 
platform: Android
updatedAt: Tue Dec 11 2018 01:33:04 GMT+0000 (UTC)
---
# Improve Experience Under Poor Network Conditions
## Introduction
The audio and video quality of a live broadcast or a video chat will deteriorate under unreliable network conditions. To improve the efficiency of a live broadcast or a video chat, the `setLocalPublishFallbackOption` and `setRemoteSubscribeFallbackOption` interfaces are added. These interfaces allow the SDK to automatically disable the video stream when the network conditions cannot support both audio and video, and enable the video when the network conditions improve. `onLocalPublishFallbackToAudioOnly` or `onRemoteSubscribeFallbackToAudioOnly` is triggered when the stream falls back to audio-only or when the stream switches back to the video.

## Implementation

```Java
    // Enable the dual-stream mode
    rtcEngine.enableDualStreamMode(true);

    // Configurationa for the publisher
    // When the network condition is poor, send video stream only. 
    rtcEngine.setLocalPublishFallbackOption(Constants.STREAM_FALLBACK_OPTION_AUDIO_ONLY);

    // Configurations for the subscriber.
    // Try to receive low stream under poor network conditions. When the current network conditions are not sufficient for video streams, receive audio stream only. 
    rtcEngine.setRemoteSubscribeFallbackOption(Constants.STREAM_FALLBACK_OPTION_AUDIO_ONLY);

    // To switch between the high stream and low stream of a remote video stream:
    // Switch the remote video stream with a specified uid to low stream. 
    // Note that you have to set the remote stream to dual-stream mode before receiving its low stream. 
    rtcEngine.setRemoteVideoStreamType(uid, Constants.VIDEO_STREAM_LOW);
```

## API Reference
* [enableDualStreamMode](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a645cb7d0f3a59dda27b157cf130c8c9a)
* [setLocalPublishFallbackOption](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac8c08e79844a4e62e0670553484cbe90)
* [setRemoteSubscribeFallbackOption](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af64301ea1788dad0561aa678f3fe6ad3)
* [setRemoteVideoStreamType](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a51756b4d2e7997fbe6481d2deb5c0396)



