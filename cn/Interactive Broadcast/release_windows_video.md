
---
title: 发版说明
description: 
platform: Windows
updatedAt: Fri Mar 29 2019 03:56:15 GMT+0000 (UTC)
---
# 发版说明

本文提供 Agora 视频 SDK 的发版说明。

## **简介**

Windows 视频 SDK 支持两种主要场景:

-   音视频通话
-   音视频直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video?platform=All%20Platforms) 以及[互动直播产品概述](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)了解关键特性。


## **2.3.3 版**

该版本于 2019 年 1 月 24 日发布。功能改进与修复问题详见下文。

### **改进**

为提升用户体验，屏幕共享做了大量算法优化。针对不同的共享场景提供不同的屏幕共享策略，尤其针对 PPT 翻页和网页浏览场景，提升了屏幕共享过程中的流畅度和清晰度。同时改善了通信模式下屏幕共享开启阶段画面模糊的现象。

### **问题修复**

修复了 `onNetworkQuality` 回调不准确的问题。


## **2.3.2 版**

该版本于 2019 年 1 月 16 日发布。新增特性与修复问题详见下文。

#### **升级必看**

2.3.2 除了下文提到的功能和改进外，整体提升直播模式下视频弱网下抗丢包能力，提高流畅度，降低卡顿率。升级前，请了解版本兼容性:

- Native SDK 版本号须大于等于 1.11 版本
- Web SDK 版本号须大于等于 2.1 版本

如果你的 Native SDK 是由 v2.0.8 升级至 v2.3.2，Agora 建议你另外参考[升级指南](../../cn/Interactive%20Broadcast/migration_windows.md)了解主要 API 的变动。

### **新增功能**

#### 1. 视频自采集 (Push 模式）

为方便用户在通话或直播中使用外部视频数据，该版本新增如下接口，支持使用 Push 方式进行视频自采集。启用后，应用程序需要将外部的视频帧封装成  `AgoraVideoFrame` 格式后，推送给 Agora SDK 进行编码和传输。该方法适用于用户在发送端自己做采集、渲染，然后把视频帧发送给 Agora SDK 进行编码和传输的场景。本地渲染需要用户自己完成。

- [`setExternalVideoSource`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)：配置外部视频源
- [`pushVideoFrame`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)：推送外部视频帧

#### 2. 音频自渲染（Pull 模式）

为提升外部音频源的使用体验，该版本新增如下接口，支持使用 Pull 方式进行音频自渲染。启用后，应用程序会采用主动拉取的方式从音频引擎拉取远端已解码混音后的音频帧，用于外部音频播放。

- [`setExternalAudioSink`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638)： 设置外部音频自渲染
- [`pullAudioFrame`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940)：拉取音频帧用于外部播放

和语音观测器中的 [`onPlaybackAudioFrame`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac) 方法相比，该方法的优势在于：

- 使用 `onPlaybackAudioFrame` 方法时，SDK 主动每 10 ms 回调数据给应用程序。如果应用程序处理有延时，就有可能会导致音频播放抖动；
- 使用 `pullAudioFrame` 方法时，SDK 主动拉取音频帧。通过设置 **AudioFrame** 中的 `samples` 参数，SDK 可以指定需要的播放采样数量；同时 SDK 内部会通过调整缓存，帮助应用程序处理延时，从而有效避免因外部音频播放抖动导致的异常，如音频不同步等问题。Agora 推荐使用该方法。

#### 3. 提升直播清晰度 

Agora SDK 会根据网络条件进行码率自适应。为满足用户在直播场景下对视频清晰度的要求，该版本在 [setVideoEncoderConfiguration](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) 接口中新增 [minBitrate](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#ab3249ca88227aaf36f21681b9de7ced2) 参数，强制视频编码器输出高质量图片。如果将参数设为高于默认值，在网络状况不佳情况下可能会导致网络丢包，并影响视频播放的流畅度。因此如非对画质有特殊需求，Agora 建议不要修改该参数的值。

#### 4. 控制音乐文件的播放音量

为方便用户控制混音音乐文件的播放音量，该版本在已有 [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a5e117be71d38d813208198f4064aa964) 的基础上新增 [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085) 和 [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734) 接口，用于分别控制混音音乐文件在本地和远端的播放音量。

该版本梳理了用户在音频采集到播放过程中可能会需要调整音量的场景，及各场景对应的 API，供用户参考使用。详见官网文档[调整通话音量](../../cn/Interactive%20Broadcast/volume_windows.md)。

#### 5. 直播中弱网环境下视频自动回退/重开

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec) 和 [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7) 两个接口。 用户设置这两个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或直接关闭视频流，从而保证或提高音频质量。同时 SDK 会持续监控网络质量，并在网络质量改善时恢复音视频流。在推流回退为音频流时，或由音频流恢复为音视频流，触发 [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513) 或 [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74) 回调。

#### 6. 按用户返回音视频上下行码率、帧率、丢包率及延迟

为方便统计每个用户的音视频上下行码率、帧率及丢包率，该版本新增 [`onRemoteAudioTransportStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d) 和 [`onRemoteVideoTransportStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f) 回调。 通话或直播过程中，当用户收到远端用户发送的音视频数据包后，会周期性地发生该回调上报，频率约为 2 秒 1 次。 回调中包含用户的 UID、音/视频接收码率、丢包率、以及延迟时间（毫秒）。 并在统计频道内通话相关数据的 Rtcstats 类中增加 [`lastmileDelay`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a255549c958a1dcb0aad1eb0f13ff7f18) 参数，返回客户端到 vos 服务器的延迟。

#### 7. 设置视频编码属性

为满足场景中视频旋转的需要，提升自定义视频源画质，该版本引入 [`setVideoEncoderConfiguration`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) 替换原 [`setVideoProfile`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb) 接口，来设置视频编码属性。 新接口中的 [`VideoEncoderConfiguration`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) 类对应一套视频参数，包含视频的分辨率 `dimension`、帧率 `frame rate`、码率 `bitrate`、最低编码码率`minBitrate` 以及视频方向 `orientationMode`，其中 Agora 建议保留 `minBitrate` 的默认设置。原接口 `setVideoProfile` 仍可使用，但不再推荐。

#### 8. 虚拟声卡采集

为提升声卡采集易用性，该版本在 [`enableLoopbackRecording`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a065f485fd23b8c24a593680a47d754aa) 接口中新增参数 deviceName，支持用户使用虚拟声卡进行采集。该参数 NULL 时默认使用当前声卡采集。如需使用虚拟声卡，直接使用虚拟声卡的产品名传参即可。


### **改进**

#### 1. 提供更透明的质量数据统计

为提升质量透明的用户体验，该版本废弃了原有的 `onAudioQuality` 回调，并新增 `onRemoteAudioStats` 回调进行取代。和原来的接口相比，新接口使用更为综合的算法，通过引入音频丢帧率、端到端的音频延迟、接收端网络抖动的缓冲延迟等参数，使回调结果更贴近用户感受。同时，该版本优化了 `onNetworkQuality` 的算法，对上下行网络质量采用不同的计算方法，使评分更精准。

- [`onRemoteAudioStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)：通话中远端音频流的统计信息回调。用于替换	`onAudioQuality`
- [`onNetworkQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80003ae8cce02039f3aa0e8ffad7deed)：通话中每个用户的网络上下行 Last mile 质量报告回调。

Agora SDK 计划在下一个版本对如下 API 进行进一步改进：

- [`onLastmileQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33)：通话前网络上下行 Last mile 质量报告回调

该版本对数据统计相关回调进行了统一梳理，相关回调及算法详见[通话中数据统计](../../cn/Interactive%20Broadcast/in_call_statistics_windows.md)。

#### 2. 改进获取 SDK 网络连接状态的生成策略

为提升 SDK 使用数据统计的准确性和合理性，该版本新增如下接口，用以获取 SDK 的网络连接状态，以及连接状态发生改变的原因。

- [`getConnectionState`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362)：获取 SDK 的网络连接状态
- [`onConnectionStateChanged`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)：SDK 的网络连接状态已改变回调

该版本废弃了原有的 [`onConnectionInterrupted`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483) 和 [`onConnectionBanned`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404) 回调。

在新的接口下，SDK 共有 5 种连接状态：未连接、正在连接、已连接、正在重新建立连接和连接失败。当连接状态发生改变时，都会触发 `onConnectionStateChanged` 回调。当条件满足时，原有的 `onConnectionInterrupted` 和 `onConnectionBanned` 回调也会触发，但 Agora 不再推荐使用。


#### 3. 优化打分反馈机制

为方便用户（开发者）收集最终用户（应用程序使用者）对使用应用进行通话或直播的反馈，该版本将 [`rate`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a748c30a6339ec9798daa0d1b21585411) 接口中的打分范围缩减为 1 - 5，减少最终用户的打分干扰。Agora 建议在应用程序中集成该接口，方便应用程序收集用户反馈。


#### 4. 音频设备热插拔逻辑优化，提升音频可用性

优化了通话中和直播中的设备插拔选择，避免因用户主动切换设备而导致音频无声。

#### 5. 其他优化

- 优化了直播模式下视频弱网抗丢包能力
- 加快了严重拥塞状态视频的恢复速度
- 提升了推流稳定性
- 优化了 API 的调用线程
- 增加了重新检测耳机插入和蓝牙设备连接的代码
- 降低了音频延时
- 优化了屏幕共享的性能

### **问题修复**

#### SDK 相关：

- 修复了检测设备时出现的闪退问题

#### 音频相关：

- 修复了设备在连接蓝牙的状态下，退出频道后，音频不走蓝牙的问题
- 修复了调用 `startAudioMixing` 播放音乐文件时出现的崩溃问题
- 修复了麦克风在禁用的状态下，设备插上耳机后，禁用失效的问题
- 修复了外放条件下，上下麦、系统电话打断、进退频道等多种场景下，出现的无法调节外放音量的问题
- 修复了应用从后台切回前台时，出现的出声音慢的问题
- 修复了音频模块的崩溃问题

#### 视频相关：

- 修复了因视频编码问题引起的 Native 设备与 Web 端互通时，Web 端看不到 Native 端视频画面的问题
- 修复了 x86 设备上自采集图像时硬件编码器的相关问题
- 修复了视频自采集时的偶现问题
- 修复了 Windows 设备上出现的屏幕共享时鼠标位置问题
- 修复了首帧出图时间过长的问题
- 修复了共享桌面指定区域过程中，调用 `setVideoProfile` 设备视频编码属性时，出现的共享画面左右颠倒的问题
- 修复了屏幕共享时出现的本地和远端鼠标位置不一致的问题
- 修复了使用高分辨率视频屏幕共享时出现的卡糊的问题


### **API 整理**

为提升用户体验，Agora 在 v2.3.2 版本中对 API 进行了如下变动：

#### 新增

- [`setVideoEncoderConfiguration`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e)
- [`setExternalVideoSource`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)
- [`pushVideoFrame`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)
- [`setExternalAudioSink`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638)
- [`pullAudioFrame`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7)
- [`getConnectionState`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734)
- [`onConnectionStateChanged`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)
- [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513)
- [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74)
- [`onRemoteAudioStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)
- [`onRemoteAudioTransportStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d)
- [`onRemoteVideoTransportStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f)

#### 废弃

- [`setVideoProfile`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb)
- [`onAudioQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a36ad42975f3545382de07875016fb7fa)
- [`onConnectionInterrupted`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483)
- [`onConnectionBanned`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404)

## **2.2.2 版**

该版本于 2018 年 6 月 21 日发布。修复问题列表详见下文。

### **修复问题**

- 修复了偶发的线上统计崩溃的问题
- 修复了特定场景下偶发的视频窗口尺寸变化后，视频卡住的问题
- 修复了偶发的无法正常反馈频道内谁在说话以及说话者的音量的问题

## **2.2.1 版**

该版本于 2018 年 5 月 30 日发布。内部代码优化。

## **2.2.0 版**

该版本于 2018 年 5 月 4 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

为更好地提升用户体验，Agora SDK 在 2.1 版本中对动态秘钥进行了升级。 如果你当前使用的 SDK 是 2.1 之前的版本，并希望升级到 2.1 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。

### **新增功能**

本次发版新增如下功能：

#### 1. 音效混响进频道

播放音效 `playEffect` 接口新增了一个 `publish` 参数，用于在播放音效时，远端用户可以听到本地播放的音效。

> 如果你的 SDK 是由之前版本升级到 v2.2 版本，请务必关注该接口功能的变动。

#### 2. 服务端部署代理服务器

通过 Agora 部署的代理服务器，方便有企业防火墙的用户设置代理服务器，以使用 Agora 的服务。详见 [企业部署代理服务器](../../cn/Quickstart%20Guide/proxy.md) 中的描述。

修复了连麦后退出频道后再进入频道连麦对端看不到自己的问题。

#### 3. 获取远端视频状态

新增 `onRemoteVideoStateChanged` 接口，以获知远端视频流的状态。

#### 4. 视频水印

> 在本地直播及旁路直播中增加水印功能，允许用户将一张 PNG 图片作为水印添加到正在进行的本地直播或旁路直播中。新增 `addVideoWatermark` 和 `clearVideoWatermarks` 接口，以添加或删除本地直播水印；`LiveTranscoding` 接口中新增 `watermark` 参数，用于控制旁路直播中水印的添加。

### **改进功能**

本次发版改进如下功能：

#### 1. 当前说话者音量提示

改进 `enableAudioVolumeIndication` 接口的功能，无论频道内是否有人说话，都会在回调中按设置的时间间隔返回说话者音量提示。

#### 2. 频道内网络质量监测

根据用户对频道内实时网络质量监测的需求，在 `onNetworkQuality` 中改进了返回数据的准确度。

#### 3. 进入频道前网络条件监测

为方便用户在进频道前检查当前网络是否能支撑语音或视频通话，在 `onLastmileQuality` 中，由通过恒定码率监测优化为根据用户设定的 Video Profile 的码率进行监测，提高返回数据的准确度。且在网络状态为 unknown 时，依然以 2 秒的间隔返回回调。

#### 4. 提升音乐场景下的音质

提升了用户在播放音乐等场景下的音乐音质。

### **修复问题**

-   修复了大量用户同时直播连麦时，偶发的抖屏现象

-   修复了 Windows 设备在直播模式下偶发的音频卡顿问题


## **2.1.3 版**

该版本于 2018 年 4 月 19 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

该版本的 SDK 修改了 `setVideoProfile` 方法在直播模式下的码率值，修改后的码率值与 2.0 版本一致。

### **问题修复**

修复了部分手机上，用户离开频道后，开启自带的录音设备时，偶现录音出错的问题。

### **改进**

改进了通信和直播模式下屏幕共享的效果，缩短了用户从屏幕共享模式切换回普通模式需要的时间间隔。

## **2.1.2 版**

该版本于 2018 年 4 月 2 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

SDK 升级至 2.1.2 的直播模式后，相同分辨率下，视频更清晰，但带宽也会变大。

### **问题修复**

-   修复了连麦后退出频道后再进入频道连麦对端看不到自己的问题。

-   修复了通信模式的屏幕共享清晰度小于直播模式的问题。


## **2.1.1 版**

该版本于 2018 年 3 月 16 日发布。

请正在或已集成 2.1 SDK 的客户尽快升级更新！ 本次发版修复了一个的系统风险，请尽快升级以免对服务造成影响。

## **2.1.0 版**

该版本于 2018 年 3 月 7 日发布。新增特性与修复问题列表详见下文。

### **新增功能**

本次发版新增如下功能：

#### 1. 开黑

新增了一个游戏开黑场景，用于节省流量和去除杂音，通过调用 API `setAudioProfile` 实现。

#### 2. 音效均衡和音效混响

在直播场景下，主播如果需要通过内置的麦克风美化和定制自己的语音输入，可以通过调用 API `setLocalVoiceEqualization `和 `setLocalVoiceReverb` 轻易地设置音效均衡和混响来实现所需要的效果。

#### 3. 在线频道信息查询

新增 Restful API 查询用户在频道中的状态信息，查询指定频道内的分角色用户列表，查询厂商频道列表，查询用户是否为连麦用户等。

#### 4. 17 人视频

在直播场景下，同一频道内支持 17 位主播同时进行视频直播和连麦，详见:

-   [实现视频直播](../../cn/Quickstart%20Guide/broadcast_video_windows.md)

-   [实现七人以上视频通话](../../cn/Quickstart%20Guide/seventeen_people_windows.md)


#### 5. 插入外部视频源

直播场景下，可以将采集到的视频添加到正在进行的直播中，直播室里的主播和观众可以一起边看电影、比赛或演出，边进行点评、互动等功能，会让现有的直播话题更广、体验更好。 仅支持拉入一路流，格式包括: RTMP, HLS, FLV。赛事直播最多同时支持 5 人连麦直播。详见 [外部输入直播视频源](../../cn/Quickstart%20Guide/inject_stream_windows.md) 。


#### 6. 新增直播场景下的屏幕共享功能

-   在 2.1.0 以前: Agora SDK 仅支持视频通话场景下的屏幕共享功能;

-   从 2.1.0 开始: Agora SDK 正式支持直播场景下的屏幕共享功能;


#### 7. 声卡采集

新增 API `enableLoopbackRecording` 开启视声卡采集，开启后 SDK 可以采集到本地播放的所有声音。

### **改进**

本次发版改进如下功能：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>改进</td>
<td>描述</td>
</tr>
<tr><td>卡顿</td>
<td>改善了观众模式下的卡顿现象</td>
</tr>
<tr><td>卡顿</td>
<td>改善了特定设备导致的卡顿现象</td>
</tr>
<tr><td>鉴权模型优化</td>
<td>旧鉴权模型一个密钥只能对应一个服务权限(例如加入频道)，而新的一个 Token 包含了所有的服务权限(例如加入频道，连麦，推流等)</td>
</tr>
<tr><td>计费优化</td>
<td>计费系统针对极小分辨率统一按音频计费，例如 16 * 16</td>
</tr>
</tbody>
</table>



### **问题修复**

-   修复了摄像头采集失败问题;

-   修复了偶现的崩溃;

-   修复了偶现的视频卡顿问题;



## **2.0 版及之前**
### **2.0 版**

该版本于 2017 年 12 月 6 日发布。新增特性与修复问题列表详见下文。

#### **升级必看**

致各位开发者:

由于 Agora 的产品升级， 当 Windows SDK 升级至 2.0 将不支持 Agora Recording Server（<=1.8.2）的调用接口，相关 API 将被废弃。

**受影响范围**

-   使用 Windows SDK 并且使用 Agora Recording Server（<= version 1.8.2）的用户
-   涉及到的 API 为 `startRecordingService`，`stopRecordingService` 以及 `refreshRecordingServiceStatus`，将在后续版本中不再支持。

**解决方案**

我们提供了两种方案供您选择:

方案一: 将 Agora Recording Server（<= version 1.8.2）升级到 Agora Recording SDK\( \>= version 1.12\)。Agora Recording SDK 无需在客户端触发录制，将不影响 Windows SDK 的后续升级（推荐该方法）

附上 Agora Recording SDK 的使用方法:

-   [集成 SDK](../../cn/Recording/recording_integrate_cpp.md)
-   [开始录制](../../cn/Recording/recording_cmd_cpp.md)
-   [录制 API](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/recording_cpp/index.html)


方案二：如果你希望继续使用 Agora Recording Server，维持 Windows SDK 版本不变（<=v1.14），将不影响您继续使用 Windows SDK的 API 触发录制。

若您有任何疑问，可以通过以下方式获得技术支持:

1.  通过 Agora 开发者社区提问: [https://dev.agora.io/cn/](https://dev.agora.io/cn/)
2.  通过 Agora 工单系统提交工单
3.  您还可以随时联系技术支持群里的同事或商务


#### **新增功能**

-   通信场景支持视频大小流功能，新增 API `setRemoteVideoStreamType` 和 `enableDualStreamMode` ;

-   通信和直播场景下支持音频自采集功能，新增以下 API:

 <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>setExternalAudioSource</code></td>
<td>设置外部音频采集参数: 采样率，通道数等</td>
</tr>
<tr><td><code>pushExternalAudioFrame</code></td>
<td>推送外部音频帧</td>
</tr>
</tbody>
</table>


-   通信和直播场景下支持服务端踢人功能。如有需要，请联系 [sales@agora.io](mailto:sales@agora.io) 开通该功能。

-   支持 Windows 64 位;

-   增加音量、静音设置/获取接口以及系统音量、静音同步引擎事件接口, 详见:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>setPlaybackDeviceVolume</code></td>
<td>设置播放设备音量</td>
</tr>
<tr><td><code>getPlaybackDeviceVolume</code></td>
<td>获取播放设备音量</td>
</tr>
<tr><td><code>setRecordingDeviceVolume</code></td>
<td>设置麦克风音量</td>
</tr>
<tr><td><code>getRecordingDeviceVolume</code></td>
<td>获取麦克风音量</td>
</tr>
<tr><td><code>setPlaybackDeviceMute</code></td>
<td>静音播放设备</td>
</tr>
<tr><td><code>getPlaybackDeviceMute</code></td>
<td>获取播放设备静音状态</td>
</tr>
<tr><td><code>setRecordingDeviceMute</code></td>
<td>静音麦克风</td>
</tr>
<tr><td><code>getRecordingDeviceMute</code></td>
<td>获取麦克风静音状态</td>
</tr>
<tr><td><code>onAudioDeviceVolumeChanged</code></td>
<td>当放音、录音、应用程序音量发生变化时触发该回调</td>
</tr>
</tbody>
</table>




#### **问题修复**

修复了加入频道后出现的崩溃。

### **1.14 版**

该版本于 2017 年 10 月 20 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `setAudioProfile `设置音频参数和应用场景。

-   新增 API `setLocalVoicePitch` 提供基础变声功能。

-   直播场景: 新增 API `setInEarMonitoringVolume` 提供调节耳返音量功能。


#### **改进**

-   优化了在高码率下的音频体验。

-   秒开: 直播场景下，单流模式时观众加入频道 1 秒内看见主播图像\(均值为 885 ms, 网络状态良好时可达 788 ms\)。

-   节省带宽:

    -   1.14 以前: 如果你选择不听某人的音频或不看某人的视频，音视频流会照发。

    -   1.14 开始: 如果你选择不听或不看某人的流，则不会下发，从而节省带宽。

-   精准的码率控制:

    -   1.14 以前: 码率控制不够精准，上下波动幅度较大。波动过大容易造成网络拥塞，增加丢包、丢帧的概率，影响了带宽估计模块的精度，特别是在弱网低码率情况下尤为明显。

    -   1.14 开始: 精准的码率控制，要多少给多少，不多给也不少给，避免波动过大造成的网络拥塞，减少传输延时，有助于减少网络卡顿。


#### **问题修复**

-   修复了部分 Windows 机器上无声音的问题。

-   修复了部分 Windows 机器上的摄像头问题。


### **1.13.1 版**

该版本于 2017 年 9 月 28 日发布。新增特性与修复问题列表详见下文。

#### **优化**

优化了特定场景下出现的回声问题。

### **1.13 版**

该版本于 2017 年 9 月 4 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `onClientRoleChanged` 用于提醒直播场景下主播、观众上下麦的回调。

-   新增单独关闭语音播放的功能。

-   新增功能支持服务端推流失败回调。

-   屏幕共享直播场景下采集声卡选项动态开启和关闭。


#### **改进**

-   软编情况下，视频属性可控。

-   可以在客户端设置推流的 Profile。

-   屏幕共享: 提升了画质清晰度和流畅度。

-   屏幕共享: 通信场景下新增动态更新截图区域。


#### **修复问题**

修复了部分机型上偶现的崩溃。

### **1.12 版**

该版本于 2017 年 7 月 25 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   直播场景下， 新增 API 方法 `injectStream` 在当前频道内插入一条 RTMP 流。该功能目前为 beta 版

-   在 API 方法 `setEncryptionMode` 里新增加密模式 `aes-128-ecb` 。

-   在 API 方法 `startAudioRecording` 里新增参数 `quality`用于设置录音音质。

-   新增 API `onActiveSpeaker` 提示当前频道内谁在说话。

-   通信场景下，删除了原有的 API 方法 `setScreenCaptureWindow`，更新 API 方法 `startScreenCapture` 共享整个屏幕、指定窗口或指定区域

-   通信场景下，启用屏幕共享功能后，在屏幕共享过程中可以显示鼠标


#### **改进**

通信场景下针对 320 x 180 分辨率提供了以下改进方案:

-   网络和设备状态较差的情况下仍能保证画质流畅度。

-   网络和设备状态良好的情况下可以做到比 180P 更好的画质清晰度。


#### **修复问题**

修复了部分机型上偶现的崩溃问题。


