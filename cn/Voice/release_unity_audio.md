
---
title: 发版说明
description: 
platform: Unity
updatedAt: Mon May 25 2020 07:57:59 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora Unity SDK 的发版说明。

## 简介
 
Agora Unity SDK 主要支持两种主要场景：
 
- 音频通话
- 音频直播
 
点击[语音通话产品概述](../../cn/Voice/product_voice.md)及[音频互动直播产品概述](../../cn/Voice/product_live_audio.md)了解关键特性。

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

#### 3. 原始音频数据

该版本支持原始音频数据，你可以获取原始音频数据并自行处理。

#### 4. 音频自采集

该版本提供音频自采集的接口，你可以配置外部音频源及推送音频数据。

#### 5. 加密

该版本支持加密功能，你可以对音频流进行加密。下表展示移动端的加密库信息，若希望减小 SDK 体积且不使用加密功能，你可以把加密库移除。

   | 平台    | 加密库                                     |
   | :------ | :----------------------------------------- |
   | Android | libagora-crypto.so                         |
   | iOS     | <ul><li>AgoraRtcCryptoLoader.framework <li>libcrypto.a</li></ul> |

#### 6. 云代理服务

支持使用云代理服务，方便部署企业防火墙的用户正常使用 Agora 的服务，详见[使用云代理服务](../../cn/Voice/cloudproxy_unity.md)。

**相关文档**

你可以参考以下文档集成 SDK，实现相应的实时音频功能：

- [实现语音通话](../../cn/Voice/start_call_audio_unity.md)或[实现语音直播](../../cn/Voice/start_live_audio_unity.md)
- [API 参考](https://docs.agora.io/cn/Voice/API%20Reference/unity/index.html)

Agora 提供了开源的 [Unity GitHub Sample](https://github.com/AgoraIO/Agora-Unity-Quickstart/tree/master/audio/Hello-Unity3D-Agora)，你也可以前往下载并体验。
