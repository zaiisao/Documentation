
---
title: Improve Expereience Under Poor Network Conditions
description: 
platform: Windows
updatedAt: Wed Nov 21 2018 10:38:49 GMT+0000 (UTC)
---
# Improve Expereience Under Poor Network Conditions
## Introduction

The audio and video quality of a live broadcast or a video chat will deteriorate under unreliable network conditions. To improve the efficiency of a live broadcast or a video chat, the `setLocalPublishFallbackOption` and `setRemoteSubscribeFallbackOption` interfaces are added. These interfaces allow the SDK to automatically disable the video stream when the network conditions cannot support both audio and video, and enable the video when the network conditions improve. `onLocalPublishFallbackToAudioOnly` or `onRemoteSubscribeFallbackToAudioOnly` is triggered when the stream falls back to audio-only or when the stream switches back to the video.
