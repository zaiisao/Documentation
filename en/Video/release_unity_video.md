
---
title: Release Notes
description: 
platform: Unity
updatedAt: Wed Jul 08 2020 16:02:00 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Unity SDK.

## Overview

The Agora Unity SDK supports the following scenarios:

-   Voice or video call
-   Live interactive audio or video streaming

For the key features included in each scenario, see [Agora Voice Call Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Agora Video Call Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), [Agora Live Interactive Audio Streaming Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms) and [Agora Live Interactive Video Streaming Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

## v2.9.2

v2.9.2 is released on Feb 17, 2020.

- This release fixed some abnormal behaviors on Android devices.
- This release fixed the stuck behavior of using the Editor debug mode on Windows platform.

## v2.9.1

Agora Unity SDK is widely used in games, education, AR, VR and other scenarios.

v2.9.1 was released on December 23, 2019.

**Functions and features**

#### 1. Multi-platform support
Supports iOS, Android, macOS and Windows (x86/x86_64) platforms.

#### 2. Interoperability with the Agora Web SDK
Provides the `EnableWebSdkInteroperability` method for enabling interoperability with the Agora Web SDK in `LIVE_BROADCASTING` profile. 

#### 3. Video rendering method
Supports multiple video rendering methods. You can choose any method in  **Auto Graphics API**.
![](https://web-cdn.agora.io/docs-files/1576826628073)

#### 4. Multithreaded rendering
Supports multithreaded rendering. You can click the **Multithreaded Rendering** option for rendering in multiple threads.

#### 5. Raw data
Supports raw audio data and raw video data in RGBA format. You can capture and process the raw data according to your needs. See details in [Raw Video Data](../../en/Video/raw_data_video_unity.md).

#### 6. External data source
Provides APIs for accessing to the external data source. You can configure the external audio and video source, and push the data to the Agora Unity SDK. See details in [Custom Video Source and Renderer](../../en/Video/custom_video_unity.md)

#### 7. Encryption
Supports encryption of audio and video streaming. The following table shows the information of the encryption libraries for the Android and iOS platforms. If you do not intend to use this function, you can remove the encryption libraries to decrease the SDK size.

   | Platform | Encryption libraries                          |
   | :------- | :-------------------------------------------- |
   | Android  | libagora-crypto.so                            |
   | iOS      | <ul><li>AgoraRtcCryptoLoader.framework <li>libcrypto.a</li></ul> |

#### 8. Cloud proxy

Supports the cloud proxy service. See [Use Cloud Proxy](../../en/Video/cloudproxy_unity.md) for details.

**Related documentation**

See the following documentation to quickly integrate the SDK and implement real-time voice and video communication in your project.

- [Start a Video Call](https://docs.agora.io/en/Video/start_call_unity?platform=Unity) or [Start Live Interactive Video Streaming](https://docs.agora.io/en/Interactive%20Broadcast/start_live_unity?platform=Unity)
- [API Reference](https://docs.agora.io/en/Video/API%20Reference/unity/index.html) 

Agora also provides an open-source [Unity Sample](https://github.com/AgoraIO/Agora-Unity-Quickstart/tree/master/video/Hello-Video-Unity-Agora) on GitHub.
