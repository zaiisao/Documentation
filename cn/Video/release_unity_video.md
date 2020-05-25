
---
title: 发版说明
description: 
platform: Unity
updatedAt: Mon May 25 2020 07:56:48 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora Unity SDK 的发版说明。

## 简介
 
Agora Unity SDK 主要支持两种场景：
 
- 音视频通话
- 音视频直播
 
点击[语音通话产品概述](../../cn/Video/product_voice.md)、[视频通话产品概述](../../cn/Video/product_video.md)、[音频互动直播产品概述](../../cn/Video/product_live_audio.md)及[视频互动直播](../../cn/Video/product_live.md)了解关键特性。

## **2.9.2 版**

该版本于 2020 年 2 月 17 日发布。

- 该版本修复了 Android 设备上的部分异常。
- 该版本修复了 Windows 平台下，使用 Editor 调试模式时偶现的卡死问题。

## **2.9.1 版**

Agora Unity SDK 广泛应用于游戏、教育、AR、VR 等场景。

该版本于 2019 年 12 月 23 日发布。功能特性及相关文档详见下文。

**功能特性**

#### 1. 多平台支持

该版本支持在 iOS、Android、macOS 和 Windows (x86/x86_64) 平台中集成。

#### 2. 支持与 Web 互通

该版本提供 `EnableWebSdkInteroperability` 方法，用于打开直播场景下与 Agora Web SDK 的互通。

#### 3. 视频渲染方式

该版本支持多样的视频渲染方式，你可以在 Unity 菜单中选择 **Auto Graphics API** 下任意的视频渲染模式。
![](https://web-cdn.agora.io/docs-files/1576826628073)

#### 4. 多线程渲染

该版本支持多线程渲染，你可以在 Unity 菜单中选择 **Multithreaded Rendering** 启用该功能。

#### 5. 原始音视频数据

该版本支持原始音频数据和 RGBA 格式的原始视频数据，你可以获取原始音视频数据并自行处理。详见[原始视频数据](../../cn/Video/raw_data_video_unity.md)。

#### 6. 音视频自采集

该版本提供音视频自采集的接口，你可以配置外部音视频源及推送音视频数据。详见[自定义视频采集和渲染](../../cn/Video/custom_video_unity.md)。

#### 7. 加密

该版本支持加密功能，你可以对音视频流进行加密。下表展示移动端的加密库信息，若希望减小 SDK 体积且不使用加密功能，你可以把加密库移除。

   | 平台    | 加密库                                     |
   | :------ | :----------------------------------------- |
   | Android | libagora-crypto.so                         |
   | iOS     | <ul><li>AgoraRtcCryptoLoader.framework <li>libcrypto.a</li></ul> |

#### 8. 云代理服务

支持使用云代理服务，方便部署企业防火墙的用户正常使用 Agora 的服务，详见[使用云代理服务](../../cn/Video/cloudproxy_unity.md)。

**相关文档**

你可以参考以下文档集成 SDK，实现相应的实时音视频功能：

- [实现视频通话](https://docs.agora.io/cn/Video/start_call_unity?platform=Unity)或[实现视频直播](https://docs.agora.io/cn/Interactive%20Broadcast/start_live_unity?platform=Unity)
- [API 参考](https://docs.agora.io/cn/Video/API%20Reference/unity/index.html)

Agora 提供了开源的 [Unity GitHub Sample](https://github.com/AgoraIO/Agora-Unity-Quickstart/tree/master/video/Hello-Video-Unity-Agora)，你也可以前往下载并体验。
