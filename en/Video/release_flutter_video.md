
---
title: Release Notes
description: 
platform: Flutter
updatedAt: Wed Sep 30 2020 12:02:58 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Flutter SDK.

## Overview

The Agora Flutter SDK wraps the Agora RTC SDKs for Android and iOS, so you can quickly implement real-time communication functionality in your Android and iOS apps developed using the Flutter framework.

The Agora Flutter SDK supports the following scenarios:
- Voice and Video Call
- Live Interactive Audio and Video Streaming

For key features included in each scenario, see the following documentation:
- [Agora Voice Call Overview](https://docs.agora.io/en/Voice/product_voice)
- [Agora Video Call Overview](https://docs.agora.io/en/Video/product_video)
- [Agora Interactive Audio Streaming Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio)
- [Agora Interactive Video Streaming Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live)

### v3.1.2

v3.1.2 was released on September 30th, 2020.

**Functions and features**

- Adapts to Agora RTC SDK v3.1.2.
- Supports asynchronous programming with `async/await`.
- Supports multiple channels.
- Supports TextureView on Android platform.
- Supports callbacks to report the current publishing and subscribing states, and whether the first audio or video frame is published.

**Related documentation**

See the following documentation to quickly integrate the SDK and implement real-time voice and video communication in your project.
- [Start a Voice Call](../../en/Video/start_call_audio_flutter.md)
- [Start a Video Call](../../en/Video/start_call_flutter.md)
- [Start Live Interactive Video Streaming](../../en/Video/start_live_flutter.md)
- [Start Live Interactive Audio Streaming](../../en/Video/start_live_audio_flutter.md)
- [API Reference](https://docs.agora.io/en/Video/API%20Reference/flutter/v3.1.2/index.html)

Agora also provides an open-source [Agora Flutter Quickstart](https://github.com/AgoraIO-Community/Agora-Flutter-Quickstart) on GitHub.

<div class="alert note">To improve user experience, this version disables the local network connection quality report by default, to prevent the prompt to find local network devices from popping up when an end user launches the app on an iOS 14.0 device. The <code>gatewayRtt</code> parameter in the <code>RtcStats</code> callback is disabled by default. Do not use <code>gatewayRtt</code> to obtain the round-trip delay between the client and the local router. See the <a href="https://docs.agora.io/en/faq/local_network_privacy">FAQ</a> for details.</div>
