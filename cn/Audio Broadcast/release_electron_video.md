
---
title: 发版说明
description: 
platform: Electron
updatedAt: Fri May 22 2020 04:08:34 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora SDK for Electron 的发版说明。

## 简介
 
Agora SDK for Electron 基于 Agora SDK for macOS 和 Agora SDK for Windows，使用 Node.js C++ 插件开发，支持其它平台 Native SDK 的所有功能。主要支持两种主要场景：
 
- 音视频通话
- 音视频直播
 
点击[语音通话产品概述](../../cn/Audio%20Broadcast/product_voice.md)、[视频通话产品概述](../../cn/Audio%20Broadcast/product_video.md)、[音频互动直播产品概述](../../cn/Audio%20Broadcast/product_live_audio.md)及[视频互动直播](../../cn/Audio%20Broadcast/product_live.md)了解关键特性。

## **3.0.0 版**

该版本于 2020 年 4 月 7 日发布。

在该版本对通信场景采用了全新的系统架构，并升级了通信和直播场景下的 last mile 网络策略。在带宽不足时，新的网络策略能充分利用上下行有限带宽提升有效码率，从而增强弱网对抗能力，极大提升了弱网情况下通信和直播场景的终端用户体验。

由于通信场景采用了新的系统架构，为保证新老版本通信用户的互通兼容，我们使用了回退机制。如果频道内有老版本通信用户加入，则当前版本 (3.0.0) 的终端用户会回退成老版本通信。一旦回退，频道内所有用户都无法享受新版本带来的体验提升。因此我们强烈推荐同步升级所有终端用户到当前版本。

同时，我们对本地服务端录制进行了升级发布。为确保享受全新架构和网络策略优化带来的好处，使用本地服务端录制的客户，请务必同步升级本地服务端录制 SDK 至 3.0.0 版本。

新增特性、改进与问题修复详见下文。

**升级必看**

#### 1. 通信场景上行默认不开启视频小流

从该版本起，Agora 在通信场景下，默认不开启视频[小流](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-duala双流模式)。如需启用，请在成功加入频道后，调用 [`enableDualStreamMode (true)`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#enabledualstreammode) 方法启用视频双流模式。在多人视频通信场景下，我们建议你开启视频双流。

**新增特性**

#### 1. 多频道管理

为方便用户在同一时间加入多个频道，该版本新增了 [`AgoraRtcChannel`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcchannel.html) 类。通过创建多个 `AgoraRtcChannel` 对象，用户可以同时加入各 `AgoraRtcChannel` 对象对应的频道中，实现多频道功能。

加入多个频道后，用户可以同时接收多个频道的流，但只能同时在一个频道内发流。该功能适用于用户需要同时接收多个频道的流，或频繁切换频道发流的场景。

#### 2. 调节本地播放的指定远端用户音量

该版本新增 [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#adjustuserplaybacksignalvolume) 方法，用以调节本地用户听到的指定远端用户的音量。通话或直播过程中，你可以多次调用该方法，来调节多个远端用户在本地播放的音量，或对某个远端用户在本地播放的音量调节多次。

#### 3. 人声检测

为判断本地用户是否说话，该版本在启用说话者音量提示 [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#enableaudiovolumeindication) 方法中新增 `boolean` 型的 `report_vad` 参数。启用该参数后，你会在 `groupAudioVolumeIndication` 回调中获取本地用户的人声状态。

**改进**

#### 1. 音频编码属性

为满足更高音质需求，该版本调整了直播场景下 [`setAudioProfile`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#setaudioprofile) 中 `profile(0)` 对应的音频编码属性，详见下表：

| SDK 版本   | profile(0)                                                 |
| :--------- | :----------------------------------------------------------- |
| 3.0.0      | 48 KHz 采样率，音乐编码，单声道，编码码率最大值为 52 Kbps。  |
| 3.0.0 之前 | <li>macOS：32 KHz 采样率，音乐编码，单声道，编码码率最大值为 44 Kbps。</li><li>Windows：32 KHz 采样率，音乐编码，单声道，编码码率最大值为 64 Kbps。</li> |

#### 2. 镜像模式

为提升视频镜像的使用体验，该版本增加了视频编码镜像的功能：

在 [`VideoEncoderConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/videoencoderconfiguration.html) 接口中，新增 `mirrorMode` 属性，方便设置本地视频编码的镜像模式，即远端看本地是否镜像。

#### 3. 质量透明

为方便开发者获取更多通话统计信息，该版本在 [`RtcStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/rtcstats.html) 接口中新增 `gatewayRtt`、`memoryAppUsageRatio`、`memoryTotalUsageRatio` 和 `memoryAppUsageInKbytes` 属性，方便更好地监控通话中的网络状态和内存使用情况。

#### 4. 屏幕共享

为支持更多屏幕共享使用场景，该版本新增支持通过窗口信息共享屏幕时支持共享[通用 Windows 平台](https://docs.microsoft.com/zh-cn/windows/uwp/get-started/universal-application-platform-guide)（UWP）应用窗口。

#### 5. 直播水印

该版本允许用户将一张 PNG 图片作为水印添加到正在进行的本地直播中。新增 [`addVideoWatermark`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#addvideowatermark) 和 [`clearVideoWatermark`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#clearvideowatermarks) 方法，以添加或删除本地直播水印。

#### 6. 设置客户端录音采样率

为方便用户设置客户端录音的采样率，该版本废弃了原有方法，并使用新的 [`startAudioRecording`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#startaudiorecording) 方法进行取代。新的方法下，录音采样率可设为 16、32、44.1 或 48 kHz。

#### 7. 其他提升

该版本自动开启直播场景下 Electron SDK 与 Web SDK 的互通，并废弃原有的 `enableWebSdkInteroperability` 和 `videoSourceEnableWebSdkInteroperability` 方法。

**问题修复**

- 修复了混音、音频录制、音频编码、回声等音频问题。
- 修复了水印、视频画面比例、画质模糊、视频不能全屏、屏幕共享黑边等视频问题。
- 修复了特定场景下偶现的 app 崩溃、日志文件、推流不稳定等问题。
- 通信场景下，调用 [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#setremotesubscribefallbackoption) 方法也生效。
- 一对一通信场景下，下行音视频弱网下会回退为纯音频。
- macOS 10.15 系统下，偶现系统渲染窗口 UI 异常。

**API 变更**

#### 行为变更

该版本修改了 macOS 设备连接耳机或蓝牙时的音频路由。修改后的语音路由与 macOS 设备管理器中显示的一致。

#### 新增

- [`VideoEncoderConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/videoencoderconfiguration.html) 接口新增 `mirrorMode` 属性
- [`createChannel`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#createchannel)
- [`AgoraRtcChannel`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcchannel.html) 类
- [`RtcStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/rtcstats.html) 接口中新增 `gatewayRtt`、`memoryAppUsageRatio`、`memoryTotalUsageRatio` 和 `memoryAppUsageInKbytes` 属性
- [`startAudioRecording`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#startaudiorecording) 
- [`addVideoWatermark`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#addvideowatermark) 
- [`clearVideoWatermark`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#clearvideowatermarks) 
- [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#enableaudiovolumeindication) ，新增 `report_vad` 参数

#### 废弃

- `enableWebSdkInteroperability`
- `videoSourceEnableWebSdkInteroperability`
- `firstRemoteVideoFrame`，使用 `remoteVideoStateChanged` 取代
- `userMuteAudio`,`firstRemoteAudioDecoded` 和 `firstRemoteAudioFrame`，使用 `remoteAudioStateChanged` 取代
- `streamPublished` 和 `streamUnpublished`，使用 `rtmpStreamingStateChanged` 取代

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

为方便直播频道中的观众用户快速切换到其他频道，该版本新增 [`switchChannel`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#switchchannel) 方法。和先调 `leaveChannel`，再调 `joinChannel` 相比，该方法能实现更快的频道切换。调用 `switchChannel` 方法切换到其他直播频道后，本地会先收到离开原频道的回调 `leaveChannel`，再收到成功加入新频道的回调 `joinedChannel`。

#### 2. 跨频道媒体流转发

跨频道媒体流转发，指将主播的媒体流转发至其他直播频道，实现主播跨频道与其他主播实时互动的场景。该版本新增如下接口，通过将源频道中的媒体流转发至目标频道，实现跨直播间连麦功能：

- [`startChannelMediaRelay`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#startchannelmediarelay)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#updatechannelmediarelay)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#stopchannelmediarelay)

在跨频道媒体流转发过程中，SDK 会通过 `channelMediaRelayState` 和 `channelMediaRelayEvent` 回调报告媒体流转发的状态和事件。

#### 3. 本地及远端音频状态

为方便用户了解本地及远端的音频状态，该版本新增 `localAudioStateChanged` 和 `remoteAudioStateChanged` 回调。新的回调下，本地及远端音频有如下状态：

- 本地音频：`STOPPED(0)`、`RECORDING(1)`、`ENCODING(2)` 和 `FAILED(3)`。状态为 `FAILED(3)` 时，你可以通过 `error` 参数中返回的错误码定位及排查问题。
- 远端音频：`STOPPED(0)`、`STARTING(1)`、`DECODING(2)`、`FROZEN(3)` 和 `FAILED(4)`。你可以在 `reason` 参数中了解引起远端音频状态发生改变的原因。

#### 4. 本地音频数据

为方便更好地了解通话质量，获取更多质量相关数据，该版本新增 `localAudioStats` 回调，通过 `numChannels`、`sentSampleRate`、`sentBitrate` 参数报告本地音频统计信息。

**改进**

#### 1. 通话中质量透明

该版本进一步扩充了 [`RtcStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/rtcstats.html)、[`LocalVideoStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/localvideostats.html) 和 [`RemoteVideoStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/remotevideostats.html) 类的成员。各类新增成员如下：

- `RtcStats` 类：累计发送音频/视频字节数及累计接收音频/视频字节数
- `LocalVideoStats` 类：本地视频的编码码率、宽高、发送帧数及编码类型
- `RemoteVideoStats` 类：远端视频在网络对抗后的丢包率

#### 2. 直播视频质量提升

该版本改善了弱网条件下直播视频卡顿问题，提升了画面清晰度，优化了网络极端丢包情况下的直播画面流畅度。

#### 3. 屏幕共享质量提升

该版本提升了通信场景下，下行网络状况不佳时，屏幕共享文字的清晰度。该改进只在屏幕共享的内容类型 `ContentHint` 设置为 `Details(2)` 时生效。

#### 4. 其他改进

- 优化了 Game Streaming 模式下的音频质量。
- 优化了通信场景下用户关闭麦克风后听到的音质。

**问题修复**

#### 音频

- 修复了与 Web 互通时听声辨位过程中出现的声音失真的问题。
- 修复了 [`muteRemoteAudioStream`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#muteremoteaudiostream) 方法调用无效的问题。
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
- [`switchChannel`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#switchchannel)
- [`startChannelMediaRelay`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#startchannelmediarelay)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#updatechannelmediarelay)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#stopchannelmediarelay)
- `channelMediaRelayState`
- `channelMediaRelayEvent`
- `remoteVideoStateChanged` ：`reason` 和 `elapsed`，原有的参数 `state` 使用新的枚举类取代
- [`RtcStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/rtcstats.html) 类：`txAudioBytes`，`txVideoBytes`，`rxAudioBytes` 和 `rxVideoBytes` 成员
- [`localVideoStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/localvideostats.html) 类：`encodedBitrate`，`encodedFrameWidth`，`encodedFrameHeight`，`encodedFrameCount` 和 `codecType` 成员
- [`remoteVideoStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/interfaces/remotevideostats.html) 类：`packetLossRate` 成员


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
 
- 集成 SDK：[实现音视频通话](../../cn/Audio%20Broadcast/start_call_electron.md)或[实现互动直播](../../cn/Audio%20Broadcast/start_live_electron.md)
- [API 参考](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/electron/index.html)
 
Agora 提供了开源的 [Electron GitHub Demo](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart)，你也可以前往下载并体验。
