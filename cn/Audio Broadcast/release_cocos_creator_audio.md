
---
title: 发版说明
description: 
platform: Cocos Creator
updatedAt: Fri Nov 06 2020 14:51:19 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora Cocos Creator SDK 的发版说明。

## 简介

Agora Cocos Creator SDK 基于 Agora iOS SDK 和 Agora Android SDK 开发，支持 Agora iOS SDK 和 Agora Android SDK 对应版本的所有功能。主要支持以下两种场景：

- 语音通话
- 音频互动直播

点击[语音通话产品概述](../../cn/Audio%20Broadcast/product_voice.md)和[音频互动直播产品概述](../../cn/Audio%20Broadcast/product_live_audio.md)了解关键特性。

## 3.1.2 版

该版本于 2020 年 10 月 30 日发布。

**主要特性**

#### 1. 支持移动端平台

该版本支持在 Android 和 iOS 平台中集成。

#### 2. 支持与 Web 互通

该版本自动开启与 Agora Web SDK 的互通。

#### 3. 加密

该版本支持加密功能，你可以对音频流进行加密。下表展示 Android 和 iOS 平台的加密库信息。如果希望减小 SDK 体积且不使用加密功能，你可以把加密库移除。

| 平台    | 加密库                           |
| :------ | :------------------------------- |
| Android | `libagora-crypto.so`             |
| iOS     | `AgoraRtcCryptoLoader.framework` |

#### 4. 云代理服务

该版本支持使用云代理服务，方便部署企业防火墙的用户正常使用 Agora 的服务。

#### 5. 区域访问限制

该版本支持 `initWithAreaCode` 方法，你可以在初始化 Agora 引擎时指定服务器的访问区域。该功能为高级设置，适用于有访问安全限制的场景。目前支持的区域有中国大陆、北美、欧洲、亚洲（中国大陆除外）、日本、印度和全球（默认）。

**相关文档**

你可以参考以下文档集成 SDK，实现相应的实时音频功能：

- [实现语音通话](../../cn/Audio%20Broadcast/start_call_audio_cocos_creator.md)或[实现音频互动直播](../../cn/Audio%20Broadcast/start_live_audio_cocos_creator.md)
- [API 参考](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cocos_creator_voice/index.html)

Agora 提供了开源的 [Voice-Call-for-Mobile-Gaming](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-CocosCreator-Voice-Agora) 示例项目，你可以前往下载并体验。
