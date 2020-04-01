
---
title: 加入频道前的 API 调用
description: configurtions before joinChannel
platform: All Platforms
updatedAt: Wed Apr 01 2020 07:02:13 GMT+0800 (CST)
---
# 加入频道前的 API 调用
本文介绍在加入频道（`joinChannel`）之前的 API 调用和设置。在加入频道前，通常只需要简单调用一两个 API 即可快速实现实时音视频功能，但是如果你的应用场景对通话质量和稳定性有较高的要求，我们建议参考本文进行更多的设置。

如无特别说明，本文内容适用于 Agora RTC SDK 以下平台：
- Android
- iOS
- macOS 
- Windows
- Electron
- Unity

<div class="alert note">阅读本文前请确保你已经了解如何实现一个<a href="https://docs.agora.io/cn/Video/start_call_android?platform=Android">音视频通话</a>或<a href="https://docs.agora.io/cn/Interactive%20Broadcast/start_live_android?platform=Android">直播</a>。</div>

## 日志文件设置

为了确保 SDK 输出的日志信息完整，方便调查问题，我们建议在引擎初始化之后立即调用 `setLogFile` 设置日志文件，另外可以配合 `setLogFileSize`、`setLogFilter` 设置日志文件大小和输出等级，详见[如何设置日志文件](https://docs.agora.io/cn/faqs/logfile)。

## 音频设置

如果你的场景有高音质需求（例如音乐教学场景），建议在加入频道前调用 `setAudioProfile`，并将 `profile` 参数设置为 `MUSIC_HIGH_QUALITY`(4)，`scenario` 参数设置为 `GAME_STREAMING`(3)，更多的设置可以参考[设置音频属性](https://docs.agora.io/cn/Interactive%20Broadcast/audio_profile_android?platform=Android)。

<div class="alert note">不同平台的参数枚举值可能略有不同，请根据实际情况进行调整。</div>

## 与 Web 平台互通

如果你的应用场景中包括 Web 端，并且你使用的 Native SDK 版本在 3.0.0 之前，直播模式下，一定要在加入频道前调用 `enableWebSdkInteroperability` 方法打开和 Web 端的互通。

<div class="alert info">从 3.0.0 版本开始无需调用该方法，Native SDK 默认打开和 Web 端的互通。</div>
