
---
title: 发版说明
description: 
platform: macOS
updatedAt: Wed Jul 10 2019 06:47:56 GMT+0800 (CST)
---
# 发版说明

本文提供 Agora 语音 SDK 的发版说明。

## **简介**

macOS 语音 SDK 支持两种主要场景:

-   音频通话
-   音频直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms) 以及 [音频互动直播产品概述](https://docs.agora.io/cn/Audio%20Broadcast/product_live_audio?platform=All%20Platforms)了解关键特性。

## **2.8.0 版**

该版本于 2019 年 7 月 8 日发布。新增特性与修复问题列表详见下文。

### **新增特性**

#### 1. 全平台支持 String 型的用户名

很多 App 使用 String 类型的用户名。为降低开发成本，Agora 新增支持 String 型的 User account，方便用户通过如下接口直接使用 App 账号加入 Agora 频道：

- [registerLocalUserAccount](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)

对于其他接口，Agora 沿用 Int 型的 UID。Agora Engine 会维护 UID 和 User account 映射表，你可以随时通过 String user account 获取 UID，或者通过 UID 获取 String user account，无需自己维护映射表。

为保证通信质量，频道内所有用户需使用同一数据类型的用户名，即频道内的所有用户名应同为 Int 型或同为 String 型。详见[使用 String 型的用户名](../../cn/Audio%20Broadcast/string_mac.md)。

**Note**：

- 同一频道内，Int 型的 User ID 和 String 型的 User account 不可混用。目前支持 String 型 User account 的 SDK 如下：

	- Native SDK：v2.8.0 及之后版本
	- Web SDK：v2.5.0 及之后版本

 如果你的频道内有不支持 String 型 User account 的用户，则只能使用 Int 型的 User ID。
- 如果你使用该版本的 Native SDK 将用户名升级至 String 型 User account，请确保所有终端用户同步升级。
- 如果使用 String 型的 User account，请确保你的服务端用户生成 Token 的脚本已升级至最新版本。你可以使用 String 型或 Int 型的用户名生成 Token。如果使用 String 型 User account 加入频道，请确保也使用该 User account 来生成 Token。你也可以使用 User account 对应的 Int 型 UID 来生成 Token。你可以调用 `getUserInfoByUserAccount` 来获取 User account 所对应的 UID。

#### 2. 音频卡顿回调

为监控通话过程中的音频传输质量，方便开发者客观体验通信质量，该版本在远端音频统计数据 [AgoraRtcRemoteAudioStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) 类中新增 `totalFrozenTime` 和 `frozenRate` 成员，报告远端用户在加入频道后发生音频的卡顿时长及卡顿率。

同时，该版本在 [AgoraRtcRemoteAudioStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) 类中还新增 `numChannels`、`receivedSampleRate` 和 `receivedBitrate` 成员。

### **改进**

为方便开发者统计掉线率，该版本在 [connectionChangedToState](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) 回调的 `AgoraConnectionChangedReason` 参数中添加 `AgoraConnectionChangedKeepAliveTimeout(14)` 成员，表示 SDK 与服务器连接保活超时，引起 SDK 连接状态发生改变。

### **修复问题**

- 特定场景下偶现的崩溃问题。

### **API 变更**

为提升用户体验，Agora 在 v2.8.0 版本中对 API 进行了如下变动：

#### 新增

- [registerLocalUserAccount](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)
- [getUserInfoByUid](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUid:withError:)
- [getUserInfoByUserAccount](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUserAccount:withError:)
- [didRegisteredLocalUser](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRegisteredLocalUser:withUid:)
- [didUpdatedUserInfo](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didUpdatedUserInfo:withUid:)
- [AgoraRtcRemoteAudioStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) 类中新增 `numChannels`，`receivedSampleRate`，`receivedBitrate`，`totalFrozenTime` 和 frozenRate 成员

#### 废弃

- [AgoraLiveTranscoding](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html) 类中的 `lowLatency` 成员


## **2.4.1 版**

该版本于 2019 年 6 月 12 日发布。

该 SDK 首次发版。你可以参考以下文档集成 SDK，实现相应的实时音频功能：

- [快速集成](../../cn/Audio%20Broadcast/mac_video.md)
- [校验用户权限](../../cn/Audio%20Broadcast/token.md)
- [检测通话质量](../../cn/Audio%20Broadcast/in_call_statistics_macos_audio.md)
- [调整通话音量](../../cn/Audio%20Broadcast/volume_mac.md)
- [播放音效/音乐混音](../../cn/Audio%20Broadcast/effect_mixing_mac.md)
- [变声与混响](../../cn/Audio%20Broadcast/voice_effect_mac.md)
- [推流到 CDN](../../cn/Audio%20Broadcast/push_stream_ios2.0_audio.md)
- [音频设备测试与切换](../../cn/Audio%20Broadcast/switch_audio_device_mac.md)

如果你是由之前版本的 macOS 完整包升级到当前的纯音频包，可参考 [macOS 完整包发版说明](../../cn/Audio%20Broadcast/release_mac_video.md)了解音频的相关改进。
