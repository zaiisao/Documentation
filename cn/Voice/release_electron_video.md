
---
title: 发版说明
description: 
platform: Electron
updatedAt: Tue Jan 21 2020 08:22:27 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora SDK for Electron 的发版说明。

## 简介
 
Agora SDK for Electron 基于 Agora SDK for macOS 和 Agora SDK for Windows，使用 Node.js C++ 插件开发，支持其它平台 Native SDK 的所有功能。主要支持两种主要场景：
 
- 音视频通话
- 音视频直播
 
点击[语音通话产品概述](../../cn/Voice/product_voice.md)、[视频通话产品概述](../../cn/Voice/product_video.md)、[音频互动直播产品概述](../../cn/Voice/product_live_audio.md)及[视频互动直播](../../cn/Voice/product_live.md)了解关键特性。

 ## **2.9.0 版**

该版本于 2019 年 8 月 30 日发布。新增特性与修复问题详见下文。

**升级必看**

#### 远端视频状态

为方便用户了解远端视频状态，该版本删除了原有的 `remoteVideoStateChanged` 接口，并使用一个新的同名接口进行取代。新的接口下， `state` 参数扩展为 `STOPPED(0)`、`STARTING(1)`、`DECODING(2)`、`FROZEN(3)` 和 `FAILED(4)`。同时，新接口还增加了 `reason` 参数，用以报告远端视频状态发生改变的原因。因此，如果你将 SDK 升级至该版本，请确保重新实现 `remoteVideoStateChanged` 接口。

同时，扩展后的 `state` 参数和新增的 `reason` 参数搭配使用，可以涵盖大部分远端视频状态，因此该版本废弃了如下接口。你可以继续使用这些接口，但我们不再推荐。详细的取代方案，请参考 API 文档：

- `userEnableVideo`
- `userEnableLocalVideo`
- `addStream`



**新增特性**

#### 1. 快速切换频道

为方便直播频道中的观众用户快速切换到其他频道，该版本新增 [`switchChannel`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#switchchannel) 方法。和先调 `leaveChannel`，再调 `joinChannel` 相比，该方法能实现更快的频道切换。调用 `switchChannel` 方法切换到其他直播频道后，本地会先收到离开原频道的回调 `leaveChannel`，再收到成功加入新频道的回调 `joinedChannel`。

#### 2. 跨频道媒体流转发

跨频道媒体流转发，指将主播的媒体流转发至其他直播频道，实现主播跨频道与其他主播实时互动的场景。该版本新增如下接口，通过将源频道中的媒体流转发至目标频道，实现跨直播间连麦功能：

- [`startChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#startchannelmediarelay)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#updatechannelmediarelay)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#stopchannelmediarelay)

在跨频道媒体流转发过程中，SDK 会通过 `channelMediaRelayState` 和 `channelMediaRelayEvent` 回调报告媒体流转发的状态和事件。

#### 3. 本地及远端音频状态

为方便用户了解本地及远端的音频状态，该版本新增 `localAudioStateChanged` 和 `remoteAudioStateChanged` 回调。新的回调下，本地及远端音频有如下状态：

- 本地音频：`STOPPED(0)`、`RECORDING(1)`、`ENCODING(2)` 和 `FAILED(3)`。状态为 `FAILED(3)` 时，你可以通过 `error` 参数中返回的错误码定位及排查问题。
- 远端音频：`STOPPED(0)`、`STARTING(1)`、`DECODING(2)`、`FROZEN(3)` 和 `FAILED(4)`。你可以在 `reason` 参数中了解引起远端音频状态发生改变的原因。

#### 4. 本地音频数据

为方便更好地了解通话质量，获取更多质量相关数据，该版本新增 `localAudioStats` 回调，通过 `numChannels`、`sentSampleRate`、`sentBitrate` 参数报告本地音频统计信息。

**改进**

#### 1. 通话中质量透明

该版本进一步扩充了 [`RtcStats`](https://docs.agora.io/cn/Voice/API%20Reference/electron/interfaces/rtcstats.html)、[`LocalVideoStats`](https://docs.agora.io/cn/Voice/API%20Reference/electron/interfaces/localvideostats.html) 和 [`RemoteVideoStats`](https://docs.agora.io/cn/Voice/API%20Reference/electron/interfaces/remotevideostats.html) 类的成员。各类新增成员如下：

- `RtcStats` 类：累计发送音频/视频字节数及累计接收音频/视频字节数
- `LocalVideoStats` 类：本地视频的编码码率、宽高、发送帧数及编码类型
- `RemoteVideoStats` 类：远端视频在网络对抗后的丢包率

#### 2. 直播视频质量提升

该版本改善了弱网条件下直播视频卡顿问题，提升了画面清晰度，优化了网络极端丢包情况下的直播画面流畅度。

#### 3. 屏幕共享质量提升

该版本提升了通信模式下，下行网络状况不佳时，屏幕共享文字的清晰度。该改进只在屏幕共享的内容类型 `ContentHint` 设置为 `Details(2)` 时生效。

#### 4. 其他改进

- 优化了 Game Streaming 模式下的音频质量。
- 优化了通信模式下用户关闭麦克风后听到的音质。

**问题修复**

#### 音频

- 修复了与 Web 互通时听声辨位过程中出现的声音失真的问题。
- 修复了 [`muteRemoteAudioStream`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#muteremoteaudiostream) 方法调用无效的问题。
- 修复了特殊场景下偶现的音频无声的问题。
- 修复了测试麦克风时出现的崩溃问题。

#### 视频
- 修复了视频卡住的问题。
- 修复了 `remoteVideoStateChanged` 回调行为不符预期的问题。


#### 其他

- 修复了偶现的旁路推流串流的问题。
- 修复了偶现的崩溃问题。
- 修复了特定场景下加入频道失败的问题。

**API 变更**

为提升用户体验，Agora SDK 在该版本中对 API 进行了如下变动：

#### 新增

- `localAudioStats`
- `localAudioStateChanged`
- `remoteAudioStateChanged`
- [`switchChannel`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#switchchannel)
- [`startChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#startchannelmediarelay)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#updatechannelmediarelay)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/electron/classes/agorartcengine.html#stopchannelmediarelay)
- `channelMediaRelayState`
- `channelMediaRelayEvent`
- `remoteVideoStateChanged` ：`reason` 和 `elapsed`，原有的参数 `state` 使用新的枚举类取代
- [`RtcStats`](https://docs.agora.io/cn/Voice/API%20Reference/electron/interfaces/rtcstats.html) 类：`txAudioBytes`，`txVideoBytes`，`rxAudioBytes` 和 `rxVideoBytes` 成员
- [`localVideoStats`](https://docs.agora.io/cn/Voice/API%20Reference/electron/interfaces/localvideostats.html) 类：`encodedBitrate`，`encodedFrameWidth`，`encodedFrameHeight`，`encodedFrameCount` 和 `codecType` 成员
- [`remoteVideoStats`](https://docs.agora.io/cn/Voice/API%20Reference/electron/interfaces/remotevideostats.html) 类：`packetLossRate` 成员


#### 废弃

- `microphoneEnabled` 回调，请改用 `localAudioStateChanged`
- `remoteVideoTransportStats`，请改用 `remoteVideoStats`
- `remoteAudioTransportStats`，请改用 `remoteAudioStats`
- `userEnableVideo`，请改用 `remoteVideoStateChanged`
- `userEnableLocalVideo`，请改用 `remoteVideoStateChanged`
- `addStream`，请改用 `remoteVideoStateChanged`

## **2.8.0 版**

该版本于 2019 年 7 月 8 日发布。
 
该 SDK 首次发版。你可以参考以下文档集成 SDK，实现相应的实时音视频功能：
 
- 集成 SDK：[实现音视频通话](../../cn/Voice/start_call_electron.md)或[实现互动直播](../../cn/Voice/start_live_electron.md)
- [API 参考](https://docs.agora.io/cn/Voice/API%20Reference/electron/index.html)
 
Agora 提供了开源的 [Electron GitHub Demo](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart)，你也可以前往下载并体验。
