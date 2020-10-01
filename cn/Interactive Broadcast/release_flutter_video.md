
---
title: 发版说明
description: 
platform: Flutter
updatedAt: Wed Sep 30 2020 12:04:53 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora Flutter SDK 的发版说明。

## 简介

Agora Flutter SDK 基于 Android 和 iOS 平台的 Agora RTC SDK 封装，可在基于 Flutter 开发的 Android 和 iOS 移动端应用中快速实现实时音视频功能。

Agora Flutter SDK 主要支持两种场景：

- 音视频通话
- 音视频直播

点击[语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video)、[音频互动直播产品概述](https://docs.agora.io/cn/Audio%20Broadcast/product_live_audio)及[视频互动直播产品概述](https://docs.agora.io/cn/Interactive%20Broadcast/product_live)了解关键特性。

## 3.1.2 版

该版本于 2020 年 9 月 30 日发布。功能特性及相关文档详见下文。


**功能特性**

1. 全面适配 Agora RTC SDK v3.1.2。
2. 支持多频道管理。
3. 全面支持异步编程 （`async/await`）。
4. Android 平台支持 `TextureView` 渲染。
5. 支持发布和订阅状态转换回调、本地首帧发布回调。

**相关文档**

你可以参考以下文档集成 SDK，实现相应的实时音视频功能：

- 快速开始：
  - [实现语音通话](../../cn/Video/start_call_audio_flutter.md)
  - [实现视频通话](../../cn/Video/start_call_flutter.md)
  - [实现视频互动直播](../../cn/Video/start_live_flutter.md)
  - [实现音频互动直播](../../cn/Video/start_live_audio_flutter.md)
- [API 参考](https://docs.agora.io/cn/Video/API%20Reference/flutter/v3.1.2/index.html)

Agora 在 GitHub 提供一个开源的 [Agora Flutter Quickstart](https://github.com/AgoraIO-Community/Agora-Flutter-Quickstart) 示例项目。你也可以前往下载并体验。

<div class="alert note">为提升用户体验，防止 app 在 iOS 14.0 版本上触发查看本地网络设备的弹窗提示，该版本默认关闭本地网络连通质量报告功能。<code>RtcStats</code> 中的 <code>gatewayRtt</code> 参数会失效，请不要使用该参数获取客户端到本地路由器的往返延时。详见 <a href="https://docs.agora.io/cn/faq/local_network_privacy">FAQ</a>。 如果你不介意弹窗提示，且需启用本地网络连通质量报告功能，请<a href="https://agora-ticket.agora.io/">提交工单</a>联系声网技术支持。</div>
