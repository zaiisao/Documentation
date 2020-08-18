
---
title: 发版说明
description: 
platform: Windows
updatedAt: Tue Aug 11 2020 10:30:31 GMT+0800 (CST)
---
# 发版说明

本文提供 Agora 语音 SDK 的发版说明。

## **简介**

Windows 语音 SDK 支持两种主要场景:

-   音频通话
-   音频直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms) 以及 [音频互动直播产品概述](https://docs.agora.io/cn/Audio%20Broadcast/product_live_audio?platform=All%20Platforms)了解关键特性。

Windows 语音 SDK 支持 x86 和 x64 架构。

## **3.1.0 版**

该版本于 2020 年 8 月 11 日发布。

**新增特性**

#### 1. 发布和订阅状态转换回调

该版本新增以下回调方便你了解音频流当前的发布及订阅状态，有助于订阅和发布相关的数据统计：

- `onAudioPublishStateChanged`：音频发布状态发生改变。
- `onAudioSubscribeStateChanged`：音频订阅状态发生改变。

#### 2. 本地首帧发布回调

为提示用户本地音频首帧已发布，该版本新增如下回调：

`onFirstLocalAudioFramePublished`：已发布本地音频首帧回调。该回调取代 `onFirstLocalAudioFrame` 回调，我们推荐你不再使用 `onFirstLocalAudioFrame` 回调。

#### 3. 自定义数据上报

该版本支持自定义数据上报。如需试用，请联系 sales@agora.io 开通并商定自定义数据格式。

**改进**

#### 1. 指定访问区域完善

该版本新增以下枚举值，在调用 `initialize` 初始化 `IRtcEngine` 时提供更多区域选择。指定访问区域后，集成了 Agora SDK 的 app 会连接指定区域内的 Agora 服务器。

- `AREA_CODE_JAPAN`：日本。
- `AREA_CODE_INDIA`：印度。

#### 2. CDN 直播推流

为提升 CDN 直播推流用户体验，该版本新增 `onRtmpStreamingEvent` 回调，报告推流过程中发生的事件，如未成功添加背景图或水印。

#### 3. 加密

该版本新增 `enableEncryption` 方法，用于开启内置加密，并废弃原加密方法：

- `setEncryptionSecret`
- `setEncryptionMode`

与原加密方法相比，该方法新增对国密 SM4 加密模式的支持，你可以根据需要选择合适的加密模式。

#### 4. 通话中质量透明

该版本进一步扩充了 `LocalAudioStats` 和 `RemoteAudioStats` 类的成员，提供更多音频质量相关数据。

- `LocalAudioStats` 类新增 `txPacketLossRate`，表示本端到 Agora 边缘服务器的物理音频丢包率 (%)。
- `RemoteAudioStats` 类中新增 `publishDuration`，表示远端音频流和视频流的累计发布时长（毫秒）。

#### 5. 设置音频编码属性

为提升音频性能，该版本对音频编码码率最大值进行如下优化：

| Profile |  3.1.0 版本 |  3.1.0 版本之前  |
|---|---|---|
| `AUDIO_PROFILE_DEFAULT`   | <li>直播场景：64 Kbps</li><li>通信场景：16 Kbps</li>   |  <li>直播场景：52 Kbps</li><li>通信场景：16 Kbps</li>  |
| `AUDIO_PROFILE_SPEECH_STANDARD`   | 18 Kbps   |  18 Kbps  |
| `AUDIO_PROFILE_MUSIC_STANDARD`   | 64 Kbps   | 48 Kbps   |
| `AUDIO_PROFILE_MUSIC_STANDARD_STEREO`  |  80 Kbps | 56 Kbps   |
| `AUDIO_PROFILE_MUSIC_HIGH_QUALITY`  |  96 Kbps  | 128 Kbps   |
| `AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO`  | 128 Kbps   |  192 Kbps  |

#### 6. 日志扩容

该版本中，Agora SDK 日志文件的默认个数由 2 个增加至 5 个，单个日志文件的默认大小由 512 KB 扩大至 1024 KB。默认情况下，SDK 会生成 `agorasdk.log`、`agorasdk_1.log`、`agorasdk_2.log`、`agorasdk_3.log`、`agorasdk_4.log` 这 5 个日志文件。最新的日志永远写在 `agorasdk.log` 中。`agorasdk.log` 写满后，SDK 会从 1-4 中删除修改时间最早的一个文件，然后将 `agorasdk.log` 重命名为该文件，并建立新的 `agorasdk.log` 写入最新的日志。

**问题修复**

该版本修复了华为特定型号的笔记本上偶现啸叫的问题。

**API 变更**

#### 新增

- [`onAudioPublishStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af5188bdc817fa62aac51b3dc627dfb64)
- [`onAudioSubscribeStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae3f517bd166a9e34aa77f9fbf137a7b9)
- [`onFirstLocalAudioFramePublished`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a03058e62a0a4a41e80ecf0279975ab99)
- [`enableEncryption`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ad5ea5f0dfd8117f38d9c4b12fe01fece)
- [`LocalAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_local_audio_stats.html) struct 中新增 `txPacketLossRate`
- [`RemoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) struct 中新增 `publishDuration`
- [`onRtmpStreamingEvent`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa2beb17562a3be9b5ddbe9366245cb0e)
- 错误码：`ERR_NO_SERVER_RESOURCES(103)`
- 警告码：`WARN_APM_RESIDUAL_ECHO(1053)`

#### 废弃

- `setEncryptionSecret`
- `setEncryptionMode`
- `onFirstLocalAudioFrame`

#### 删除

- 警告码：`WARN_ADM_IMPROPER_SETTINGS(1053)`

## **3.0.1 版**

该版本于 2020 年 5 月 27 日发布。

**新增特性**

#### 1. 调整音乐文件音调

为方便调整混音时音乐文件的播放音调，该版本新增 `setAudioMixingPitch` 方法。通过设置该方法的 `pitch` 参数，你可以升高或降低音乐文件的音调。该方法仅对音乐文件音调有效，对本地人声不生效。

#### 2. 变声与混响

为提高用户的音频体验，该版本在 `setLocalVoiceChanger` 和 `setLocalVoiceReverbPreset` 中分别新增以下枚举值：

- 在 `VOICE_CHANGER_PRESET` 枚举中新增了以 `VOICE_BEAUTY` 为前缀和以 `GENERAL_BEAUTY_VOICE` 为前缀的枚举值，分别实现美音或语聊美声功能。
- 在 `AUDIO_REVERB_PRESET` 枚举中新增了以 `AUDIO_REVERB_FX` 为前缀的枚举值和 `AUDIO_VIRTUAL_STEREO`，分别实现增强版混响效果和虚拟立体声效果。

你可以查看进阶功能[变声与混响](../../cn/Voice/voice_changer_windows.md)了解使用方法和注意事项。

#### 3. 远端音频数据后处理多频道支持

在多频道场景下，为方便后处理各频道的远端音频数据，该版本在 `IAudioFrameObserver` 类中新增 `isMultipleChannelFrameWanted` 和 `onPlaybackAudioFrameBeforeMixingEx`。

成功注册音频观测器后，如果你将 `isMultipleChannelFrameWanted` 的返回值设为 `true`，就可以通过上述回调获取多个频道对应的音频数据。在多频道场景下，我们建议你将返回值设为 `true`。


**问题修复**

- 修复了混音、Loopback 测试异常等问题。
- 修复了 [`onClientRoleChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a36d3f45184cbb37ed2c4846654a14368) 回调多次、App ID 和 Token 校验、日志目录乱码等问题。

**API 变更**

#### 新增

- [`setAudioMixingPitch`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a26b117f7e097801b03522f7da9257425)
- [`VOICE_CHANGER_PRESET`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/namespaceagora_1_1rtc.html#ae29d1fb09d785334eabf0f3def8b4117) `enum` 中新增 `AUDIO_REVERB_FX_KTV` 等 9 个枚举值
- [`AUDIO_REVERB_PRESET`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/namespaceagora_1_1rtc.html#a2476d004b44df3950ef62022cd41e564) `enum` 中新增 `VOICE_BEAUTY_VIGOROUS` 等 12 个枚举值
- [`IAudioFrameObserver`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html) 类中新增 [`isMultipleChannelFrameWanted`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a4b6bdf2a975588cd49c2da2b6eff5956) 和 [`onPlaybackAudioFrameBeforeMixingEx`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ab0cf02ba307e91086df04cda4355905b) 
- [`RemoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) 结构体中新增 `totalActiveTime` 成员
- [警告码](https://docs.agora.io/cn/Voice/API%20Reference/cpp/namespaceagora.html#a32d042123993336be6646469da251b21)中新增 `WARN_ADM_WINDOWS_NO_DATA_READY_EVENT(1040)` 和 `WARN_ADM_INCONSISTENT_AUDIO_DEVICE(1042)`

## **3.0.0.2 版**

该版本于 2020 年 4 月 22 日发布。

**新增特性**

#### 设置区域访问限制

该版本在 `RtcEngineContext` 结构体中新增 `areaCode` 成员，支持在创建 `IRtcEngine` 实例时指定服务器的访问区域。该功能为高级设置，适用于有访问安全限制的场景。目前支持的区域有中国大陆、北美、欧洲、亚洲（中国大陆除外）和全球（默认）。

指定访问区域后，集成了 Agora SDK 的 app 在指定区域使用时，正常情况下会连接指定区域的 Agora 服务器；在非指定区域使用时，会从所在区域和指定区域的服务器地址中，择优选择服务器建立连接。

**API 变更**

#### 新增

[`RtcEngineContext`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_engine_context.html) 结构体中新增 `areaCode` 成员

## **3.0.0** 版

该版本于 2020 年 3 月 5 日发布。

Agora 在该版本对通信场景采用了全新的系统架构，并升级了通信和直播场景下的 last mile 网络策略。在带宽不足时，新的网络策略能充分利用上下行有限带宽提升有效码率，从而增强弱网对抗能力，极大提升了弱网情况下通信和直播场景的终端用户体验。

由于通信场景采用了新的系统架构，为保证新老版本通信用户的互通兼容，我们使用了回退机制。如果频道内有老版本通信用户加入，则当前版本 (3.0.0) 的终端用户会回退成老版本通信。一旦回退，频道内所有用户都无法享受新版本带来的体验提升。因此我们强烈推荐同步升级所有终端用户到当前版本。

同时，我们对本地服务端录制进行了升级发布。为确保享受全新架构和网络策略优化带来的好处，使用本地服务端录制的客户，请务必同步升级本地服务端录制 SDK 至 3.0.0 版本。

新增特性、改进与问题修复详见下文。

**新增特性**

#### 1. 多频道管理

为方便用户在同一时间加入多个频道，该版本新增了 `IChannel` 和 `IChannelEventHandler` 类。通过创建多个 `IChannel` 对象，用户可以加入各 `IChannel` 对象对应的频道中，实现多频道功能。

加入多个频道后，用户可以同时接收多个频道的流，但只能同时在一个频道内发流。该功能适用于用户需要同时接收多个频道的流，或频繁切换频道发流的场景。详细的集成步骤和注意事项，请参考[加入多频道](../../cn/Voice/multiple_channel_windows.md)。

#### 2. 调节本地播放的指定远端用户音量

该版本新增 `adjustUserPlaybackSignalVolume` 方法，用以调节本地用户听到的指定远端用户的音量。通话或直播过程中，你可以多次调用该方法，来调节多个远端用户在本地播放的音量，或对某个远端用户在本地播放的音量调节多次。


**改进**

#### 1. 音频编码属性

为满足更高音质需求，该版本调整了直播场景下 `AUDIO_PROFILE_DEFAULT (0)` 对应的音频编码属性，详见下表：

| SDK 版本   | AUDIO_PROFILE_DEFAULT (0)                                   |
| :--------- | :---------------------------------------------------------- |
| 3.0.0      | 48 KHz 采样率，音乐编码，单声道，编码码率最大值为 52 Kbps。 |
| 3.0.0 之前 | 32 KHz 采样率，音乐编码，单声道，编码码率最大值为 64 Kbps。 |

#### 2. 质量透明

为方便开发者获取更多通话统计信息，该版本在 `RtcStats` 类中新增 `gatewayRtt`、`memoryAppUsageRatio`、`memoryTotalUsageRatio` 和 `memoryAppUsageInKbytes` 成员，方便更好地监控通话的质量和通话过程中的内存变动。

#### 3. 其他提升

该版本自动开启直播场景下 Native SDK 与 Web SDK 的互通，并废弃原有的 `enableWebSdkInteroperability` 方法。

**问题修复**

* 修复了混音、音频录制、音频编码、回声等音频问题。
* 修复了特定场景下偶现的 app 崩溃、日志文件、推流不稳定等问题。

**API 变更**

#### 新增

- [`AudioVolumeInfo`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) 结构体新增 [`channelId`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html#ab67471def88118f010e6d5add7d83f64) 成员
- [`createChannel`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine2.html#a9cabefe84d3a52400f941f1bd8c0f486)
- [`IChannel`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_channel.html) 类
- [`IChannelEventHandler`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_channel_event_handler.html) 类
- [`RtcStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) 类中新增[`gatewayRtt`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a30cc889ef34f63e23950b54591218cb5)、[`memoryAppUsageRatio`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#aab879ec9c52f2dd8d662c26c4939a7d3)、[`memoryTotalUsageRatio`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#aae6d57e709b08258be372960f9e19fd6) 和 [`memoryAppUsageInKbytes`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a17258e12476bb9106ccc248af4cfe734) 成员

#### 废弃

* `RtcEngineParameters` 类
* `enableWebSdkInteroperability`
* `onUserMuteAudio`, `onFirstRemoteAudioDecoded` 和 `onFirstRemoteAudioFrame`，使用 [`onRemoteAudioStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612) 取代
* `onStreamPublished` 和 `onStreamUnpublished`，使用 [`onRtmpStreamingStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d) 取代

## **2.9.1 版**
该版本于 2019 年 9 月 19 日发布。新增特性与修复问题列表详见下文。

**新增特性**

#### 人声检测

为判断本地用户是否说话，该版本在启用说话者音量提示 [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4b30a8ff1ae50c4c114ae4f909c4ebcb) 方法中新增 bool 型的 `report_vad` 参数。启用该参数后，你会在 [`onAudioVolumeIndication`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aab1184a2b276f509870c055a9ff8fac4) 回调报告的 [`AudioVolumeInfo`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) 结构体中获取本地用户的人声状态。

**改进**

#### 设置客户端录音采样率

为方便用户设置客户端录音的采样率，该版本废弃了原有的 `startAudioRecording` 方法，并使用新的同名方法进行取代。新的方法下，录音采样率可设为 16、32、44.1 或 48 kHz。原方法仅支持固定的 32 kHz 采样率，该版本继续保留原方法但我们不推荐使用。

**问题修复**

#### 其他

IAgoraRtcEngine.h 头文件中的拼写错误。

**API 变更**

为提升用户体验，Agora SDK 在该版本中对 API 进行了如下变动：

#### 新增

- [`startAudioRecording`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3c05d82c97a9d63ebda116b9a1e5ca3f)
- [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4b30a8ff1ae50c4c114ae4f909c4ebcb)，新增 `report_vad` 参数
- [`AudioVolumeInfo`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) 类，新增 `vad` 成员

#### 废弃

- `startAudioRecording`

## **2.9.0 版**

该版本于 2019 年 8 月 16 日发布。新增特性与修复问题详见下文。

**升级必看**

#### 1. RTMP 推流

该版本起，Agora 删除如下接口：

- `configPublisher`

如果你的 App 使用上述接口实现 RTMP 推流功能，请确保将 Native SDK 升级至最新版本，并改用如下接口实现推流：

- [`setLiveTranscoding`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)
- [`addPublishStreamUrl`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)
- [`removePublishStreamUrl`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)
- [`onRtmpStreamingStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d)

新的推流实现方法，详见[推流到 CDN](../../cn/Voice/cdn_streaming_windows.md)。

#### 2. 关闭/开启本地音频采集

为提高通信场景下，本地用户关闭麦克风后听到的音质，该版本在 `enableLocalAudio`(true) 后，将系统音量修改为媒体音量。调用 `enableLocalAudio`(false) 后，系统音量自动切换为通话音量。

**新增特性**

#### 1. 快速切换频道

为方便直播频道中的观众用户快速切换到其他频道，该版本新增 [`switchChannel`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab) 方法。和先调 `leaveChannel`，再调 `joinChannel` 相比，该方法能实现更快的频道切换。调用 `switchChannel` 方法切换到其他直播频道后，本地会先收到离开原频道的回调 `onLeaveChannel`，再收到成功加入新频道的回调 `onJoinChannelSuccess`。

#### 2. 跨频道媒体流转发

跨频道媒体流转发，指将主播的媒体流转发至其他直播频道，实现主播跨频道与其他主播实时互动的场景。该版本新增如下接口，通过将源频道中的媒体流转发至目标频道，实现跨直播间连麦功能：

- [`startChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)

在跨频道媒体流转发过程中，SDK 会通过 [`onChannelMediaRelayStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22) 和 [`onChannelMediaRelayEvent`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429) 回调报告媒体流转发的状态和事件。

该场景的实现方法、API 调用时序、示例代码及开发注意事项，请参考[跨直播间连麦](../../cn/Voice/media_relay_windows.md)。

#### 3. 本地及远端音频状态

为方便用户了解本地及远端的音频状态，该版本新增 [`onLocalAudioStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) 和 [`onRemoteAudioStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612) 回调。新的回调下，本地及远端音频有如下状态：

- 本地音频：STOPPED(0)、RECORDING(1)、ENCODING(2) 和 FAILED(3)。状态为 FAILED(3) 时，你可以通过 `error` 参数中返回的错误码定位及排查问题。
- 远端音频：STOPED(0)、STARTING(1)、DECODING(2)、FROZEN(3) 和 FAILED(4)。你可以在 `reason` 参数中了解引起远端音频状态发生改变的原因。

#### 4. 本地音频数据

为方便更好地了解通话质量，获取更多质量相关数据，该版本新增 [`onLocalAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3) 回调，通过 `numChannels`、`sentSampleRate`、`sentBitrate` 参数报告本地音频统计信息。

**改进**

#### 1. 通话中质量透明

该版本进一步扩充了 `RtcStats` 类的成员。新增成员如下：

- [`RtcStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) 类：累计发送音频字节数及累计接收音频字节数


#### 2. 其他改进

- 优化了 Game Streaming 模式下的音频质量。
- 优化了通信场景下用户关闭麦克风后听到的音质。

**问题修复**

#### 音频

- 修复了与 Web 互通时听声辨位过程中出现的声音失真的问题。
- 修复了 `muteRemoteAudioStream` 方法调用无效的问题。
- 修复了特殊场景下偶现的音频无声的问题。

#### 其他

- 修复了偶现的旁路推流串流的问题。
- 修复了偶现的崩溃问题。
- 修复了特定场景下加入频道失败的问题。

**API 变更**

为提升用户体验，Agora SDK 在该版本中对 API 进行了如下变动：

#### 新增

- [`onLocalAudioStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a)
- [`onRemoteAudioStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612)
- [`onLocalAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3)
- [`switchChannel`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab)
- [`startChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)
- [`onChannelMediaRelayStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22)
- [`onChannelMediaRelayEvent`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429)
- [`RtcStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) 类新增 `txAudioBytes` 和 `rxAudioBytes` 成员

#### 废弃

- `onMicrophoneEnabled`，请改用 [`onLocalAudioStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) 的 LOCAL_AUDIO_STREAM_STATE_CHANGED(0) 或 LOCAL_AUDIO_STREAM_STATE_RECORDING(1)。
- `onRemoteAudioTransportStats`，请改用 [`onRemoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)。


#### 删除

- `configPublisher`


## **2.8.0 版**

该版本于 2019 年 7 月 8 日发布。新增特性详见下文。

**新增特性**

#### 1. 全平台支持 String 型的用户 ID

很多 App 使用 String 类型的用户 ID。为降低开发成本，Agora 新增支持 String 型的 User account，方便用户通过如下接口直接使用 App 账号加入 Agora 频道：

- [registerLocalUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)

对于其他接口，Agora 沿用 Int 型的 UID。Agora Engine 会维护 UID 和 User account 映射表，你可以随时通过 String user account 获取 UID，或者通过 UID 获取 String user account，无需自己维护映射表。

为保证通信质量，频道内所有用户需使用同一数据类型的用户 ID，即频道内的所有用户 ID应同为 Int 型或同为 String 型。

**Note**：

- 同一频道内，Int 型的 User ID 和 String 型的 User account 不可混用。目前支持 String 型 User account 的 SDK 如下：

	- Native SDK：v2.8.0 及之后版本
	- Web SDK：v2.5.0 及之后版本

 如果你的频道内有不支持 String 型 User account 的用户，则只能使用 Int 型的 User ID。
- 如果你使用该版本的 Native SDK 将用户 ID升级至 String 型 User account，请确保所有终端用户同步升级。
- 如果使用 String 型的 User account，请确保你的服务端用户生成 Token 的脚本已升级至最新版本。如果使用 String 型 User account 加入频道，请确保使用该 User account 或其对应的 Int 型 UID 来生成 Token。你可以调用 `getUserInfoByUserAccount` 来获取 User account 所对应的 UID。

#### 2. 音频卡顿回调

为监控通话过程中的音频传输质量，方便开发者客观体验通信质量，该版本在远端音频统计数据 [RemoteAudioStats](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) 类中新增 `totalFrozenTime` 和 `frozenRate` 成员，报告远端用户在加入频道后发生音频的卡顿时长及卡顿率。

同时，该版本在 [RemoteAudioStats](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) 类中还新增 `numChannels`、`receivedSampleRate` 和 `receivedBitrate` 成员。

**改进**

为方便开发者统计掉线率，该版本在 [onConnectionStateChanged](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) 回调的 `reason` 参数中添加 `CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT(14)` 成员，表示 SDK 与服务器连接保活超时，引起 SDK 连接状态发生改变。

**API 变更**

为提升用户体验，Agora 在 v2.8.0 版本中对 API 进行了如下变动：

#### 新增

- [registerLocalUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)
- [getUserInfoByUid](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#abf4572004e6ceb99ce0ff76a75c69d0b)
- [getUserInfoByUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4f75984d3c5de5f6e3e4d8bd81e3b409)
- [onLocalUserRegistered](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a919404869f86412e1945c730e5219b20)
- [onUserInfoUpdated](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad086cc4d8e5555cc75a0ab264c16d5ff)
- [RemoteAudioStats](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) 类中新增 `numChannels`，`receivedSampleRate`，`receivedBitrate`，`totalFrozenTime` 和 `frozenRate` 成员

#### 废弃

- [LiveTranscoding](https://docs.agora.io/cn/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html) 类中的 `lowLatency` 成员

## **2.4.1 版**

该版本于 2019 年 6 月 12 日发布。

该 SDK 首次发版。你可以参考以下文档集成 SDK，实现相应的实时音频功能：

- [快速开始](../../cn/Voice/start_call_windows.md)
- [校验用户权限](../../cn/Voice/token.md)
- [检测通话质量](../../cn/Voice/in-call_quality_windows.md)
- [调整通话音量](../../cn/Voice/volume_windows.md)
- [播放音效/音乐混音](../../cn/Voice/audio_effect_mixing_windows.md)
- [变声与混响](../../cn/Voice/voice_changer_windows.md)
- [音频设备测试与切换](../../cn/Voice/test_switch_device_windows.md)
- [使用云代理服务](../../cn/Voice/cloudproxy_native.md)

如果你是由之前版本的 Windows 完整包升级到当前的纯音频包，可参考 [Windows 完整包发版说明](../../cn/Voice/release_windows_video.md)了解音频相关改进。

