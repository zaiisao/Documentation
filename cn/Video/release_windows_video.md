
---
title: 发版说明
description: 
platform: Windows
updatedAt: Tue May 19 2020 08:46:15 GMT+0800 (CST)
---
# 发版说明

本文提供 Agora 视频 SDK 的发版说明。

## **简介**

Windows 视频 SDK 支持两种主要场景:

-   音视频通话
-   音视频直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video?platform=All%20Platforms) 、[音频互动直播产品概述](https://docs.agora.io/cn/Audio%20Broadcast/product_live_audio?platform=All%20Platforms)以及[视频互动直播产品概述](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)了解关键特性。

Windows 视频 SDK 支持 X86 和 X64 架构。

## **3.0.0.2 版**

该版本于 2020 年 4 月 22 日发布。

**新增特性**

#### 设置区域访问限制

该版本在 `RtcEngineContext` 结构体中新增 `areaCode` 成员，支持在创建 `IRtcEngine` 实例时指定服务器的访问区域。该功能为高级设置，适用于有访问安全限制的场景。目前支持的区域有中国大陆、北美、欧洲、亚洲（中国大陆除外）和全球（默认）。

指定访问区域后，集成了 Agora SDK 的 app 在指定区域使用时，正常情况下会连接指定区域的 Agora 服务器；在非指定区域使用时，会从所在区域和指定区域的服务器地址中，择优选择服务器建立连接。

**问题修复**

该版本修复了连麦用户屏幕共享黑屏的问题。

**API 变更**

#### 新增

[`RtcEngineContext`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_engine_context.html) 结构体中新增 `areaCode` 成员

## **3.0.0** 版

该版本于 2020 年 3 月 5 日发布。

Agora 在该版本对通信场景采用了全新的系统架构，并升级了通信和直播场景下的 last mile 网络策略。在带宽不足时，新的网络策略能充分利用上下行有限带宽提升有效码率，从而增强弱网对抗能力，极大提升了弱网情况下通信和直播场景的终端用户体验。

由于通信场景采用了新的系统架构，为保证新老版本通信用户的互通兼容，我们使用了回退机制。如果频道内有老版本通信用户加入，则当前版本 (3.0.0) 的终端用户会回退成老版本通信。一旦回退，频道内所有用户都无法享受新版本带来的体验提升。因此我们强烈推荐同步升级所有终端用户到当前版本。

同时，我们对本地服务端录制进行了升级发布。为确保享受全新架构和网络策略优化带来的好处，使用本地服务端录制的客户，请务必同步升级本地服务端录制 SDK 至 3.0.0 版本。

新增特性、改进与问题修复详见下文。


**升级必看**

#### 1. 通信场景上行默认不开启视频小流

从该版本起，Agora 在通信场景下，默认不开启视频[小流](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-duala双流模式)。如需启用，请在成功加入频道后，调用 `enableDualStreamMode (true)` 方法启用视频双流模式。在多人视频通信场景下，我们建议你开启视频双流。


**新增特性**

#### 1. 多频道管理

为方便用户在同一时间加入多个频道，该版本新增了 `IChannel` 和 `IChannelEventHandler` 类。通过创建多个 `IChannel` 对象，用户可以加入各 `IChannel` 对象对应的频道中，实现多频道功能。

加入多个频道后，用户可以同时接收多个频道的流，但只能同时在一个频道内发流。该功能适用于用户需要同时接收多个频道的流，或频繁切换频道发流的场景。详细的集成步骤和注意事项，请参考《[加入多频道](../../cn/Video/multiple_channel_windows.md)》。

#### 2. 视频原始数据

为方便开发者获取传输各阶段的视频原始数据，满足更多场景需求，该版本在 `IVideoFrameObserver` 类中新增如下 C++ 回调接口：

- `onPreEncodeVideoFrame`：获取前处理后、编码前的本地视频原始数据。该方法适用于有视频前处理需求的开发场景。
- `getSmoothRenderingEnabled`：设置是否对获取的视频数据（通过 `onRenderVideoFrame`）进行平滑处理。平滑处理后的视频帧，出帧时间间隔会更均匀，因此视频自渲染的体验更好。

#### 3. 调节本地播放的指定远端用户音量

该版本新增 `adjustUserPlaybackSignalVolume` 方法，用以调节本地用户听到的指定远端用户的音量。通话或直播过程中，你可以多次调用该方法，来调节多个远端用户在本地播放的音量，或对某个远端用户在本地播放的音量调节多次。

#### 4. 美颜

常见的视频社交、在线教育和连麦直播等场景中，用户普遍希望有基础的美颜功能。该版本新增接口 setBeautyEffectOptions，你可以调用该接口设置对比度、亮度、平滑度等参数，达到美白、磨皮、红润肤色等美颜效果。详情请参考《[美颜](../../cn/Video/image_enhancement_windows.md)》。


**改进**

#### 1. 音频编码属性

为满足更高音质需求，该版本调整了直播场景下 `AUDIO_PROFILE_DEFAULT (0)` 对应的音频编码属性，详见下表：

| SDK 版本   | AUDIO_PROFILE_DEFAULT (0)                                   |
| :--------- | :---------------------------------------------------------- |
| 3.0.0      | 48 KHz 采样率，音乐编码，单声道，编码码率最大值为 52 Kbps。 |
| 3.0.0 之前 | 32 KHz 采样率，音乐编码，单声道，编码码率最大值为 64 Kbps。 |

#### 2. 镜像模式

为提升视频镜像的使用体验，该版本增加了视频编码镜像和视频渲染镜像的功能：

- 视频编码镜像：在 `VideoEncoderConfiguration` 结构体中，新增 `mirrorMode` 成员，方便设置本地视频编码的镜像模式，即远端看本地是否镜像。
- 视频渲染镜像：在 `VideoCanvas` 结构体中，新增 `mirrorMode` 成员，方便用户在调用 `setupLocalVideo` 方法初始化本地视图时，设置本地看本地是否镜像，以及调用 `setupRemoteVideo` 方法初始化远端视图时，设置本地看远端是否镜像；同时在 `setLocalRenderMode` 和 `setRemoteRenderMode` 方法中新增 `mirrorMode` 参数，支持在通话中更新本地看本地，或本地看远端的镜像模式。

#### 3. 质量透明

为方便开发者获取更多通话统计信息，该版本在 `RtcStats` 类中新增 `gatewayRtt`、`memoryAppUsageRatio`、`memoryTotalUsageRatio` 和 `memoryAppUsageInKbytes` 成员，方便更好地监控通话的质量和通话过程中的内存变动。

#### 4. 屏幕共享

为支持更多屏幕共享使用场景，该版本新增支持调用 [`startScreenCaptureByWindowId`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52) 方法时共享[通用 Windows 平台](https://docs.microsoft.com/zh-cn/windows/uwp/get-started/universal-application-platform-guide)（UWP）应用窗口。

#### 5. 其他提升

该版本自动开启直播场景下 Native SDK 与 Web SDK 的互通，并废弃原有的 `enableWebSdkInteroperability` 方法。

**问题修复**

* 修复了混音、音频录制、音频编码、回声等音频问题。
* 修复了水印、视频画面比例、画质模糊、视频不能全屏、屏幕共享黑边等视频问题。
* 修复了特定场景下偶现的 app 崩溃、日志文件、推流不稳定等问题。

**API 变更**

#### 新增

- [`setBeautyEffectOptions`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5899cc462e5250028c9afada4df98d48)
- [`onPreEncodeVideoFrame`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a2be41cdde19fcc0f365d4eb14a963e1c)
- [`getSmoothRenderingEnabled`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#aaa6c67373bb237a067318015749e8e51)
- [`setLocalRenderMode`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac433e6e88da91f87c107012cbaf8bb5c)
- [`setRemoteRenderMode`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aaaf8535dcab7ab0d12bdc351b88d09c2)
- [`VideoEncoderConfiguration`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) 结构体新增 [`mirrorMode`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#a1ca537cb6966901330bce3681194ceb5) 成员
- [`VideoCanvas`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_canvas.html) 结构体新增 [`channelId`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_canvas.html#ac16654ea954794b0e3ec8b962b4807a6)、[`mirrorMode`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_canvas.html#a7af6b90428fdcbd226f94df3cf6df9fc) 成员
- [`AudioVolumeInfo`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) 结构体新增 [`channelId`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html#ab67471def88118f010e6d5add7d83f64) 成员
- [`createChannel`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine2.html#a9cabefe84d3a52400f941f1bd8c0f486)
- [`IChannel`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_channel.html) 类
- [`IChannelEventHandler`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_channel_event_handler.html) 类
- [`RtcStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) 类中新增[`gatewayRtt`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a30cc889ef34f63e23950b54591218cb5)、[`memoryAppUsageRatio`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#aab879ec9c52f2dd8d662c26c4939a7d3)、[`memoryTotalUsageRatio`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#aae6d57e709b08258be372960f9e19fd6) 和 [`memoryAppUsageInKbytes`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a17258e12476bb9106ccc248af4cfe734) 成员

#### 废弃

* `RtcEngineParameters` 类
* `enableWebSdkInteroperability`
* `setLocalRenderMode¹`，使用新的 [`setLocalRenderMode`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac433e6e88da91f87c107012cbaf8bb5c) 取代
* `setRemoteRenderMode¹`，使用新的 [`setRemoteRenderMode`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aaaf8535dcab7ab0d12bdc351b88d09c2) 取代
* `setLocalVideoMirrorMode`，使用 [`setupLocalVideo`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a744003a9c0230684e985e42d14361f28) 和 [`setLocalRenderMode`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac433e6e88da91f87c107012cbaf8bb5c) 中的 `mirrorMode` 取代
* `onFirstRemoteVideoFrame`，使用 [`onRemoteVideoStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) 取代
* `onUserMuteAudio`, `onFirstRemoteAudioDecoded` 和 `onFirstRemoteAudioFrame`，使用 [`onRemoteAudioStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612) 取代
* `onStreamPublished` 和 `onStreamUnpublished`，使用 [`onRtmpStreamingStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d) 取代

## **2.9.3 版**

该版本于 2020 年 2 月10 日发布。

该版本修复了如下问题：

- 通信场景下，调用 `setRemoteSubscribeFallbackOption` 方法也生效。
- 一对一通信场景下，下行音视频弱网下会回退为纯音频。

## **2.9.1 版**
该版本于 2019 年 9 月 19 日发布。新增特性与修复问题列表详见下文。

**新增特性**

#### 1. 人声检测

为判断本地用户是否说话，该版本在启用说话者音量提示 [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4b30a8ff1ae50c4c114ae4f909c4ebcb) 方法中新增 bool 型的 `report_vad` 参数。启用该参数后，你会在 [`onAudioVolumeIndication`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aab1184a2b276f509870c055a9ff8fac4) 回调报告的 [`AudioVolumeInfo`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) 结构体中获取本地用户的人声状态。

#### 2. RGBA 视频原始数据

该版本新增支持 RGBA 格式的视频原始数据。你可以通过新增的 C++ 接口 [`getVideoFormatPreference`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a440e2a33140c25dfd047d1b8f7239369)，设置想要获取的视频原始数据的格式。

同时为提高开发体验，Agora 支持对 RGBA 格式的视频原始数据分别通过 C++ 接口 [`getRotationApplied`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afd5bb439a9951a83f08d8c0a81468dcb) 和 [`getMirrorApplied`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afc5cce81bf1c008e9335a0423ca45991) 进行旋转和镜像处理。

**改进**

#### 1. 直播水印

为提高直播水印的用户体验，解决视频方向模式为 `Adaptive` 时，水印位置和方向可能和预期不符的问题，该版本废弃了原有的 `addVideoWatermark` 接口，并使用一个新的同名接口进行取代。同名接口下，Agora 使用 [`WatermarkOptions`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_watermark_options.html) 类对水印进行设置，其中：

- `visibleInPreview` 成员设置本地预览是否能看见水印。
- `positionInLandscapeMode`/`positionInPortraitMode` 成员设置视频编码横屏/竖屏模式时的水印坐标。

同时，该版本对水印功能的性能进行了优化。和之前版本相比，该功能的 CPU 占用降低了 5% - 20%。

#### 2. 设置客户端录音采样率

为方便用户设置客户端录音的采样率，该版本废弃了原有的 `startAudioRecording` 方法，并使用新的同名方法进行取代。新的方法下，录音采样率可设为 16、32、44.1 或 48 kHz。原方法仅支持固定的 32 kHz 采样率，该版本继续保留原方法但我们不推荐使用。

**问题修复**

#### 其他

IAgoraRtcEngine.h 头文件中的拼写错误。

**API 变更**

为提升用户体验，Agora SDK 在该版本中对 API 进行了如下变动：

#### 新增

- [`startAudioRecording`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3c05d82c97a9d63ebda116b9a1e5ca3f)
- [`addVideoWatermark`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a36e51346ec3bd5a7370d7c70d58d9394)
- [`getVideoFormatPreference`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a440e2a33140c25dfd047d1b8f7239369)
- [`getRotationApplied`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afd5bb439a9951a83f08d8c0a81468dcb)
- [`getMirrorApplied`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afc5cce81bf1c008e9335a0423ca45991)
- [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4b30a8ff1ae50c4c114ae4f909c4ebcb)，新增 `report_vad` 参数
- [`AudioVolumeInfo`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) 类，新增 `vad` 成员

#### 废弃

- `startAudioRecording`
- `addVideoWatermark`

## **2.9.0 版**

该版本于 2019 年 8 月 16 日发布。新增特性与修复问题详见下文。

**升级必看**

#### 1. RTMP 推流

该版本起，Agora 删除如下接口：

- `configPublisher`
- `setVideoCompositingLayout`
- `clearVideoCompositingLayout`

如果你的 App 使用上述接口实现 RTMP 推流功能，请确保将 Native SDK 升级至最新版本，并改用如下接口实现推流：

- [`setLiveTranscoding`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)
- [`addPublishStreamUrl`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)
- [`removePublishStreamUrl`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)
- [`onRtmpStreamingStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d)

新的推流实现方法，详见[推流到 CDN](../../cn/Video/cdn_streaming_windows.md)。

#### 2. 远端视频状态

为方便用户了解远端视频状态，该版本删除了原有的 [`onRemoteVideoStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) 接口，并使用一个新的同名接口进行取代。新接口下， `state` 参数扩展为 STOPPED(0)、STARTING(1)、DECODING(2)、FROZEN(3) 和 FAILED(4)。同时，新接口还增加了 `reason` 参数，用以报告远端视频状态发生改变的原因。因此，如果你将 Native SDK 升级至该版本，请确保重新实现 `onRemoteVideoStateChanged` 接口。

同时，扩展后的 state 参数和新增的 reason 参数搭配使用，可以涵盖大部分远端视频状态，因此该版本废弃了如下接口。你可以继续使用这些接口，但我们不再推荐。详细的取代方案，请参考 API 文档：

- [`onUserEnableVideo`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a91bef59a3659b6e6bcbe43eb203d0732)
- [`onUserEnableLocalVideo`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a23189a2a10fb8b06b774543ac6bb322b)
- [`onFirstRemoteVideoDecoded`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a345a8441861b9dbdc7ffc36a6c6ba186)

<div class="alert note">该回调的触发时机与老的 <code>onRemoteVideoStateChanged</code> 回调不同。新接口只有在远端视频流状态发生改变时，才会触发。</div>

#### 3. 关闭/开启本地音频采集

为提高通信场景下，本地用户关闭麦克风后听到的音质，该版本在 `enableLocalAudio`(true) 后，将系统音量修改为媒体音量。调用 `enableLocalAudio`(false) 后，系统音量自动切换为通话音量。

**新增特性**

#### 1. 快速切换频道

为方便直播频道中的观众用户快速切换到其他频道，该版本新增 [`switchChannel`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab) 方法。和先调 `leaveChannel`，再调 `joinChannel` 相比，该方法能实现更快的频道切换。调用 `switchChannel` 方法切换到其他直播频道后，本地会先收到离开原频道的回调 `onLeaveChannel`，再收到成功加入新频道的回调 `onJoinChannelSuccess`。

#### 2. 跨频道媒体流转发

跨频道媒体流转发，指将主播的媒体流转发至其他直播频道，实现主播跨频道与其他主播实时互动的场景。该版本新增如下接口，通过将源频道中的媒体流转发至目标频道，实现跨直播间连麦功能：

- [`startChannelMediaRelay`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)

在跨频道媒体流转发过程中，SDK 会通过 [`onChannelMediaRelayStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22) 和 [`onChannelMediaRelayEvent`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429) 回调报告媒体流转发的状态和事件。

该场景的实现方法、API 调用时序、示例代码及开发注意事项，请参考[跨直播间连麦](../../cn/Video/media_relay_windows.md)。

#### 3. 本地及远端音频状态

为方便用户了解本地及远端的音频状态，该版本新增 [`onLocalAudioStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) 和 [`onRemoteAudioStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612) 回调。新的回调下，本地及远端音频有如下状态：

- 本地音频：STOPPED(0)、RECORDING(1)、ENCODING(2) 和 FAILED(3)。状态为 FAILED(3) 时，你可以通过 `error` 参数中返回的错误码定位及排查问题。
- 远端音频：STOPED(0)、STARTING(1)、DECODING(2)、FROZEN(3) 和 FAILED(4)。你可以在 `reason` 参数中了解引起远端音频状态发生改变的原因。

#### 4. 本地音频数据

为方便更好地了解通话质量，获取更多质量相关数据，该版本新增 [`onLocalAudioStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3) 回调，通过 `numChannels`、`sentSampleRate`、`sentBitrate` 参数报告本地音频统计信息。

**改进**

#### 1. 通话中质量透明

该版本进一步扩充了 `RtcStats`、`LocalVideoStats` 和 `RemoteVideoStats` 类的成员。各类新增成员如下：

- [`RtcStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) 类：累计发送音频/视频字节数及累计接收音频/视频字节数
- [`LocalVideoStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html) 类：本地视频的编码码率、宽高、发送帧数及编码类型
- [`RemoteVideoStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) 类：远端视频在网络对抗后的丢包率

#### 2. 直播视频质量提升

该版本改善了弱网条件下直播视频卡顿问题，提升了画面清晰度，优化了网络极端丢包情况下的直播画面流畅度。

#### 3. 屏幕共享质量提升

该版本提升了通信场景下，下行网络状况不佳时，屏幕共享文字的清晰度。该改进只在屏幕共享的内容类型 ContentHint 设置为 Details(2) 时生效。

#### 4. 摄像头热插拔

为提升 Windows 平台设备使用体验，该版本支持使用 [`RtcEngineContext`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_engine_context.html) 类中的 `context` 成员支持摄像头设备热插拔，并实现如下改进：

- 支持加入频道前或退出频道后，响应设备热插拔。
- 支持 App 视频自渲染场景下，响应设备热插拔。
- 修复了特殊场景下频繁热插拔摄像头导致的崩溃问题。
- 缩短加入频道的时间。

#### 5. 其他改进

- 优化了 Game Streaming 模式下的音频质量。
- 优化了通信场景下用户关闭麦克风后听到的音质。

**问题修复**

#### 音频

- 修复了与 Web 互通时听声辨位过程中出现的声音失真的问题。
- 修复了 `muteRemoteAudioStream` 方法调用无效的问题。
- 修复了特殊场景下偶现的音频无声的问题。

#### 视频

- 修复了 `onRemoteVideoStateChanged` 回调行为不符预期的问题。

#### 其他

- 修复了偶现的旁路推流串流的问题。
- 修复了偶现的崩溃问题。
- 修复了特定场景下加入频道失败的问题。

**API 变更**

为提升用户体验，Agora SDK 在该版本中对 API 进行了如下变动：

#### 新增

- [`onLocalAudioStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a)
- [`onRemoteAudioStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612)
- [`onRemoteVideoStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e)
- [`onLocalAudioStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3)
- [`switchChannel`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab)
- [`startChannelMediaRelay`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)
- [`onChannelMediaRelayStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22)
- [`onChannelMediaRelayEvent`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429)
- [`RtcEngineContext`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_engine_context.html) 类新增 `context` 成员
- [`RtcStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) 类新增 `txAudioBytes`，`txVideoBytes`，`rxAudioBytes` 和 `rxVideoBytes` 成员
- [`LocalVideoStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html) 类新增 `encodedBitrate`，`encodedFrameWidth`，`encodedFrameHeight`，`encodedFrameCount` 和 `codedType` 成员
- [`RemoteVideoStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) 类新增 `packetLossRate` 成员

#### 废弃

- `onMicrophoneEnabled`，请改用 [`onLocalAudioStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) 的 LOCAL_AUDIO_STREAM_STATE_CHANGED(0) 或 LOCAL_AUDIO_STREAM_STATE_RECORDING(1)。
- `onRemoteAudioTransportStats`，请改用 [`onRemoteAudioStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)。
- `onRemoteVideoTransportStats`，请改用 [`onRemoteVideoStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7163ffb650852be270ba0215b596d968)。
- `onUserEnableVideo`，请改用 [`onRemoteVideoStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) 的如下参数搭配：
	- REMOTE_VIDEO_STATE_STOPPED(0) 和 REMOTE_VIDEO_STATE_REASON_REMOTE_MUTED(5)。
	- REMOTE_VIDEO_STATE_DECODING(2) 和 REMOTE_VIDEO_STATE_REASON_REMOTE_UNMUTED(6)。

- `onUserEnableLocalVideo`，请改用 [`onRemoteVideoStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) 的如下参数搭配：
	- REMOTE_VIDEO_STATE_STOPPED(0) 和 REMOTE_VIDEO_STATE_REASON_REMOTE_MUTED(5)。
	- REMOTE_VIDEO_STATE_DECODING(2) 和 REMOTE_VIDEO_STATE_REASON_REMOTE_UNMUTED(6)。

- `onFirstRemoteVideoDecoded`，请改用 [`onRemoteVideoStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) 的 REMOTE_VIDEO_STATE_STARTING(1) 或 REMOTE_VIDEO_STATE_DECODING(2)。

#### 删除

- `configPublisher`
- `setVideoCompositingLayout`
- `clearVideoCompositingLayout`
- `onRemoteVideoStateChanged`

## **2.8.0 版**

该版本于 2019 年 7 月 8 日发布。新增特性详见下文。

**新增特性**

#### 1. 全平台支持 String 型的用户 ID

很多 App 使用 String 类型的用户 ID。为降低开发成本，Agora 新增支持 String 型的 User account，方便用户通过如下接口直接使用 App 账号加入 Agora 频道：

- [registerLocalUserAccount](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)

对于其他接口，Agora 沿用 Int 型的 UID。Agora Engine 会维护 UID 和 User account 映射表，你可以随时通过 String user account 获取 UID，或者通过 UID 获取 String user account，无需自己维护映射表。

为保证通信质量，频道内所有用户需使用同一数据类型的用户 ID，即频道内的所有用户 ID应同为 Int 型或同为 String 型。

**Note**：

- 同一频道内，Int 型的 User ID 和 String 型的 User account 不可混用。目前支持 String 型 User account 的 SDK 如下：

	- Native SDK：v2.8.0 及之后版本
	- Web SDK：v2.5.0 及之后版本

 如果你的频道内有不支持 String 型 User account 的用户，则只能使用 Int 型的 User ID。
- 如果你使用该版本的 Native SDK 将用户 ID升级至 String 型 User account，请确保所有终端用户同步升级。
- 如果使用 String 型的 User account，请确保你的服务端用户生成 Token 的脚本已升级至最新版本。如果使用 String 型 User account 加入频道，请确保使用该 User account 或其对应的 Int 型 UID 来生成 Token。你可以调用 `getUserInfoByUserAccount` 来获取 User account 所对应的 UID。

#### 2. 音视频卡顿回调

为监控通话过程中的音视频传输质量，方便开发者客观体验通信质量，该版本在远端音频统计数据 [RemoteAudioStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) 类和远端视频统计数据 [RemoteVideoStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) 类中新增 `totalFrozenTime` 和 `frozenRate` 成员，报告远端用户在加入频道后发生音视频的卡顿时长及卡顿。

同时，该版本在 [RemoteAudioStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) 类中还新增 `numChannels`、`receivedSampleRate` 和 `receivedBitrate` 成员。

**改进**

为方便开发者统计掉线率，该版本在 [onConnectionStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) 回调的 `reason` 参数中添加 `CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT(14)` 成员，表示 SDK 与服务器连接保活超时，引起 SDK 连接状态发生改变。

**API 变更**

为提升用户体验，Agora 在 v2.8.0 版本中对 API 进行了如下变动：

#### 新增

- [registerLocalUserAccount](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)
- [getUserInfoByUid](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#abf4572004e6ceb99ce0ff76a75c69d0b)
- [getUserInfoByUserAccount](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4f75984d3c5de5f6e3e4d8bd81e3b409)
- [onLocalUserRegistered](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a919404869f86412e1945c730e5219b20)
- [onUserInfoUpdated](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad086cc4d8e5555cc75a0ab264c16d5ff)
- [RemoteAudioStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) 类中新增 `numChannels`，`receivedSampleRate`，`receivedBitrate`，`totalFrozenTime` 和 `frozenRate` 成员
- [RemoteVideoStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) 类中新增 `totalFrozenTime` 和 `frozenRate` 成员

#### 废弃

- [LiveTranscoding](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html) 类中的 `lowLatency` 成员

## **2.4.1 版**

该版本于 2019 年 6 月 12 日发布。新增特性、功能改进与修复问题列表详见下文。

**升级必看**

如下内容涉及 SDK 的行为变更。如果你是由之前版本的 SDK 升级至该版本，升级前请务必阅读。

#### 1. RTMP 推流

为提高推流服务的易用性，该版本对推流接口的参数设置进行了如下限制：

| 类/接口                | 参数限制                                                     |
| -------------------------- | ------------------------------------------------------------ |
| [LiveTranscoding](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html) 类     | <li>[videoFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a8086eda68ed5b9d106cf830fd4e24f9c)：设置转码推流的帧率，单位为 fps，取值范围为 [0, 30]，默认值为 15。如果设值超过 30，Agora 服务端会自动调整为 30<li>[videoBitrate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#ab5ff17deab2e3149f149987cc0d83433)：设置转码推流的码率，单位为 Kbps，默认值为 400。用户可以根据 [Video Profile 参考表](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#af10ca07d888e2f33b34feb431300da69)中的码率值进行设置。如果设置的码率超出合理范围，服务端会在合理区间内对码率值进行自适应<li>[videoCodecProfile](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a72fbfdd2c0d4f0cc438d5ca052aa7531)：设置转码推流的视频编码规格，可设为 **BASELINE**、**MAIN** 或 **HIGH**。若设为其他值，服务端会改为默认值 **HIGH**<li>[width](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#ad0a505490500917dd246510471ef88be) 和 [height](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a340ee10a10bf0137fb996ca77729002e)：设置转码推流的视频分辨率。width x height 的最小值不低于 16 x 16</li> |
| [RtcImage](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_image.html) 类          | `url`：字符长度不得超过 **1024** 字节                        |
| [addPublishStreamUrl](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)    | `url`：字符长度不得超过 **1024** 字节                        |
| [removePublishStreamUrl](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7) | `url`：字符长度不得超过 **1024** 字节                        |

同时，该版本在 `LiveTranscoding` 类中新增 [audioCodecProfile](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a76987efee0f6d507ef648083507bd935) 参数，支持设置音频编码的规格。默认规格为 LC-AAC。

此外，该版本还对 [onStreamPublished](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a85cc73e9f53766b8a0a7fa8c96f2ea98) 方法的 `error` 参数新增了五个错误码，方便快速定位与排查问题。

#### 2、RemoteVideoStats 类参数更名

为更精准地表达远端视频流的统计信息，该版本将 [RemoteVideoStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) 类中的 `receivedFrameRate` 参数更名为 [rendererOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0)。

**新增特性**

#### 1、添加媒体附属信息

常见的直播场景中，主播给观众分发商品链接、优惠券、在线答题等，能构建更为丰富的直播互动方式。为满足该部分社交类直播 App 开发者的需求，该版本新增 [registerMediaMetadataObserver](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a84dbf06de1d769b63200d7ec0289cca0) 接口以及 [IMediaMetadataObserver](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_metadata_observer.html) 类，目前允许主播在发出的视频帧中添加 Metadata，发送媒体附属信息。

#### 2、优化屏幕共享

为确保屏幕共享画面完整性，保持共享屏幕的宽高比，避免画面裁剪和拉伸，该版本对屏幕共享的编码策略进行了优化。新版本下 Agora 按如下策略进行编码。假设 `dimensions` 值为 1920 x 1080，即 2073600 像素：

- 如果屏幕分辨率小于 `dimensions`，如 1000 x 1000，SDK 直接按 1000 x 1000 进行编码
- 如果屏幕分辨率大于 `dimensions`，如 2000 x 2000，SDK 将保持屏幕分辨率的宽高比 1：1，取 `dimensions` 以内的最大分辨率，即 1440 x 1440， 进行编码

屏幕共享按用户在 [ScreenCaptureParameters](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html) 类下设置的 [dimensions](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#a9ee8c96bbfe031e8c614d7b2e49ddbb8) 值进行计费。如果用户没有设置 `dimensions` 的值，SDK 会按 1920 x 1080 计费。

同时，为方便用户选择屏幕共享时是否采集鼠标，该版本在 `ScreenCaptureParameters` 类中新增 [captureMouseCursor](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#aa68157203616423bad85f852f3816c1f) 参数。该参数默认采集鼠标。

#### 3、本地视频状态回调

为方便开发者了解本地视频状态，该版本新增 [onLocalVideoStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) 回调。该回调下，本地视频有 `STOPPED`、`CAPTURING`、`ENCODING` 和 `FAILED` 四种状态。当本地视频状态为 `FAILED` 时，用户可以参考该回调 `error` 参数返回的错误码进行问题排查。该回调能帮助开发者辨别本地视频故障是由采集还是编码引起的。原有的 `onCameraReady` 和 `onVideoStopped` 回调在该版本废弃，我们不再推荐。

#### 4、推流状态回调

为方便推流用户了解当前的推流状态，并在推流出错时了解原因排查问题，该版本新增 [onRtmpStreamingStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d) 回调。该回调下，推流有 `IDLE`、`CONNECTING`、`RUNNING`、`RECOVERING` 和 `FAILURE` 五种状态。当推流状态为 FAILURE 时，用户可以参考该回调 `errCode` 参数返回的错误码进行问题排查。原有的 `onStreamPublished` 和 `onStreamUnpublished` 回调仍可以使用，但我们不再推荐。

#### 5、网络连接失败原因梳理

为方便开发者更好地排查网络连接相关故障，该版本梳理并整合了网络连接相关的错误码，在原有 [onConnectionStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) 回调的 `reason` 参数中新增八个导致网络连接失败的原因。新增后，只要网络连接发生错误，SDK 都会返回该回调。同时该版本废弃了原有的警告码 `WARN_LOOK_UP_CHANNEL_REJECTED(105)` 和错误码 `ERR_TOKEN_EXPIRED(109)`、`ERR_INVALID_TOKEN(110)`。

#### 6、本地网络连接类型回调

为方便用户了解本地网络的连接类型，该版本新增 [onNetworkTypeChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4) 回调。该回调下，本地网络连接有 `UNKNOWN`、`DISCONNECTED`、`LAN`、`WIFI`、`2G`、`3G`、`4G` 七种类型。当网络连接短暂中断时，该回调能帮助开发者辨别引起中断的原因是网络切换还是网络条件不好。

#### 7、获取播放伴奏音量

为方便开发者获取伴奏的播放音量，排查音量相关问题，该版本新增 [getAudioMixingPlayoutVolume](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aed9dda5a7b2683776f41f6ba0e1f281c) 和 [getAudioMixingPublishVolume](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9fafbaaf39578810ec9c11360fc7f027) 方法，用以分别获取音乐文件在本地和远端的播放音量。

#### 8、精确回调远端音频首帧解码

为更精准地获取远端用户的出声时间，该版本新增 [onFirstRemoteAudioDecoded](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7d375e7467bd099ad2d4c9cfc0a6c242) 回调，用以向 App 层报告 SDK 已完成远端音频首帧解码。在远端用户加入频道后首次发送音频，或远端用户 15 秒不发音频后再次发送时，该回调均会被触发。该回调与 `onFirstRemoteAudioFrame` 的区别在于，`onFirstRemoteAudioFrame` 在收到首个音频包时触发，先于 `onFirstRemoteAudioDecoded`。

#### 9、其他新增特性

- 该版本新增支持 64 位系统。

**改进**

#### 1、质量透明

- 该版本在通话相关的统计信息 [RtcStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) 类中，新增 [txPacketLossRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a52232c2b3af1573b3783f74f7ca75c1c) 和  [rxPacketLossRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a1882c369c5d1bc04b85fb9dcb15700b6) 参数，分别返回本地客户端到服务器和服务器到本地客户端的丢包率。
- 该版本对 LocalVideoStats 和 RemoteVideoStats 类作了如下变动，方便用户更精准地获取本地和远端视频流的统计信息：
  - [LocalVideoStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html)：新增 [encoderOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#ab5df1607ac79acbc51941797189b8dba) 和 [rendererOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#aa91d7d73f3d2c2933658a9dced6ec3ed) 参数
  - [RemoteVideoStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html)：新增 [decoderOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a21776c26b256d836a90944c1051c9322) 参数，并将原有的 receivedFrameRate 参数更名为 [rendererOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0)

#### 2、其他改进

- 优化了[AUDIO_SCENARIO_TYPE](https://docs.agora.io/cn/Video/API%20Reference/cpp/namespaceagora_1_1rtc.html#a2c5708461a9e1568bbda636d95055533) 为 GAME_STREAMING 时的音质效果
- 优化了部分场景下语音和视频的延时
- SDK 包大小降低约 0.5 M
- 提高了用户修改视频属性的码率后，网络质量打分的准确性
- 默认启用音频质量通知回调。开发者无需调用 enableAudioQualityIndication 方法，也可以收到 [onRemoteAudioStats](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a) 回调
- 提升了视频服务的稳定性
- 提升了推流服务的稳定性
- 提升了视频设备的兼容性

**问题修复**

#### 音频

- 修复了同时播放多个本地音效文件失败的问题

#### 视频

- 修复了特殊场景下发送端发小流的问题

#### 其他

- 修复了用户退出频道后仍然收到 `onNetworkQuality` 回调的问题
- 修复了偶现的崩溃问题，提升了系统稳定性

**API 变更**

为提升用户体验，Agora 在 v2.4.1 版本中对 API 进行了如下变动：

#### 全平台 C++ 接口行为一致

从该版本起，Native SDK 保证了各平台 C++ 接口的行为一致性，方便用户通过统一的 C++ 接口，在各平台保持业务逻辑一致。同时在 C++ 头文件的 `IRtcEngine` 类中实现了 `RtcEngineParameters` 类下的所有方法。各接口的适用范围及使用注意事项详见 [Agora C++ API Reference for All Platforms 首页](https://docs.agora.io/cn/Video/API%20Reference/cpp/index.html)。

#### 新增

- [getAudioMixingPlayoutVolume](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aed9dda5a7b2683776f41f6ba0e1f281c)
- [getAudioMixingPublishVolume](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9fafbaaf39578810ec9c11360fc7f027)
- [onFirstRemoteAudioDecoded](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7d375e7467bd099ad2d4c9cfc0a6c242)
- [onLocalVideoStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6)
- [onNetworkTypeChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4)
- [onRtmpStreamingStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4)
- [registerMediaMetadataObserver](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a84dbf06de1d769b63200d7ec0289cca0)
- [IMetadataObserver](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_metadata_observer.html) 类
- `LiveTranscoding` 类新增参数 [audioCodecProfile](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a76987efee0f6d507ef648083507bd935)
- `ScreenCaptureParameters` 类新增参数 [captureMouseCursor](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#aa68157203616423bad85f852f3816c1f)
- `RtcStats` 类新增参数 [txPacketLossRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a52232c2b3af1573b3783f74f7ca75c1c) 和 [rxPacketLossRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a1882c369c5d1bc04b85fb9dcb15700b6)
- `LocalVideoStats` 类新增参数 [encoderOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#ab5df1607ac79acbc51941797189b8dba) 和 [rendererOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#aa91d7d73f3d2c2933658a9dced6ec3ed)
- `RemoteVideoStats` 类新增参数 [decoderOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a21776c26b256d836a90944c1051c9322) 和 [rendererOutputFrameRate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0)（替换 receivedFrameRate）

#### 废弃

- `enableAudioQualityIndication`
- `onCameraReady`，请改用[onLocalVideoStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) 回调中的 LOCAL_VIDEO_STREAM_STATE_CAPTURING(1)
- `onVideoStopped`，请改用 [onLocalVideoStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) 回调中的 LOCAL_VIDEO_STREAM_STATE_STOPPED(0)
- 警告码 `WARN_LOOKUP_CHANNEL_REJECTED(105)`，请改用 [onConnectionStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) 回调中的 CONNECTION_CHANGED_REJECTED_BY_SERVER(10)
- 错误码 `ERR_TOKEN_EXPIRED(109)`，请改用 [onConnectionStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) 回调中的 CONNECTION_CHANGED_TOKEN_EXPIRED(9)
- 错误码 `ERR_INVALID_TOKEN(110)`，请改用 [onConnectionStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) 回调中的 CONNECTION_CHANGED_INVALID_TOKEN(8)
- 错误码 `ERR_START_CAMERA(1003)`，请改用 [onLocalVideoStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) 回调中的 LOCAL_VIDEO_STREAM_ERROR_CAPTURE_FAILURE(4)
- 错误码 `ERR_VDM_WIM_DEVICE_IN_USE(1502)`，请改用 [onConnectionStateChanged](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) 回调中的 LOCAL_VIDEO_STREAM_ERROR_DEVICE_BUSY(3)

## 2.4.0 版及之前

**2.4.0 版**

该版本于 2019 年 4 月 1 日发布。新增特性、功能改进与修复问题列表详见下文。

#### 新增功能


##### 1. 高级屏幕共享

该版本针对原有的屏幕共享进行了升级，并支持如下功能：

- 多屏环境下共享指定屏幕或屏幕内的部分区域（[`startScreenCaptureByScreenRect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a41893fe9a0ca49c054bf6dbd7d9d68f5))
- 共享指定窗口或窗口内的部分区域（[`startScreenCaptureByWindowId`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52)）
- 根据屏幕共享的内容类型设置运动优先或细节优先（[`setScreenCaptureContentHint`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aff9003c492450dbd8c3f3b9835186c95)）
- 单独设置屏幕共享的分辨率、帧率和码率（[`updateScreenCaptureParameters`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ad680e114ba3b8a0012454af6867c7498)）

该版本废弃了原有的 startScreenCapture 接口。Agora 推荐你使用新接口实现屏幕共享。新接口下，用户需要在代码中设计获取 `screenRect` 和 `windowId` 的代码逻辑，详情请参考[开始屏幕共享](../../cn/Video/screensharing_windows.md)。

##### 2. 变声和混响

在语音聊天室场景中添加变声和混响效果，能有效增强社交的趣味性。该版本在原有音效设置接口的基础上，新增 [`setLocalVoiceChanger`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a70175273dade14bcf8d74e71f8de7eda) 和 [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#add46038703f2f82dad83c0319d43433b) 方法，开发者无需手动设置音效参数，直接选择想要的本地语音变声或混响效果。

##### 3. 听声辨位

该版本新增 [`enableSoundPositionIndication`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d) 和 [`setRemoteVoicePosition`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af85b6be0a185fc67acc6eabe31467d20) 方法，支持本地用户听声辨位。用户需要在加入频道前调用 [`enableSoundPositionIndication`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d) 开启远端用户的语音立体声，然后在 [`setRemoteVoicePosition`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aab17677dfb8355c7f9269b65702549a5) 中设置远端用户声音出现的位置，通过左右耳听到的声音差异，对远端用户的声音产生方位感。在多人在线游戏场景，如射击游戏中，该功能可以增加游戏角色的方位感，模拟真实场景。

##### 4. 通话前 Last-mile 网络探测

在通话前进行 Last-mile 网络探测，可以有效帮助本地用户判断和预测上行网络质量是否良好。该版本新增通话前 Last-mile 网络探测接口 [`startLastmileProbeTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adb3ab7a20afca02f5a5ab6fafe026f2b)、[`stopLastmileProbeTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a94f3494035429684a750e1dee7ef1593) 及 [`onLastmileProbeResult`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a44134dfda5d412831fa8e44fa533fca5)，向用户反馈开始通话前上下行网络的带宽、丢包、网络抖动和往返时延数据。

##### 5. 音频设备回路测试

该版本新增音频设备回路测试接口 [`startAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac78c08f3212dc3efa000e197207dec53) 与 [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aad01da1e0bacd3f2fd355483f9e3befb)，用于测试本地的麦克风和播放设备能否正常工作。该测试在本地进行，不涉及网络传输。

##### 6. 设置用户媒体流优先级

该版本新增接口 [`setRemoteUserPriority`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aecb5d85e9b3a60947d569b88253da710) 用于设置远端用户的优先级。该方法可以与 [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7) 搭配使用。如果开启了订阅流回退选项，弱网下 SDK 会优先保证高优先级用户收到的流的质量。

##### 7. 音乐文件播放状态

该版本为播放音乐文件新增回调 [`onAudioMixingStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a298389513bfaa50af4277fc3296e3f22)，方便用户获知音乐文件的播放状态（成功/失败），以及播放出错的原因。同时新增一个警告码 701，当播放音乐文件时，本地音乐文件不存在、文件格式不支持或无法访问在线音乐文件 URL 时，均会触发该警告码。

##### 8. 设置日志文件大小

Agora SDK 有 2 个日志文件，每个文件默认大小为 512 KB。为解决该大小无法满足部分用户需求的问题，该版本新增接口 [`setLogFileSize`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a6fb256cb165856a4412bb30b098408a1)，用于设置 SDK 输出的日志文件大小。

##### 9. 直播转码支持设置背景图片

该版本在设置直播转码的 [`LiveTranscoding`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html) 类中，新增 [`backgroundImage`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a729037c7cf31b57efd1e8c9fadeab6eb) 参数，支持设置直播转码合图的背景图片。

#### 功能改进

##### 1. 质量测试与透明

- 该版本在原有的 startEchoTest 中新增参数 `intervalInSeconds`，用于设置返回测试结果的时间间隔。
- 该版本在本地视频流统计信息 [`LocalVideoStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html) 类中新增 [`targetBitrate`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#a3e8d46c9b67c9fe62487ec56bc587fd6)，[`targetFrameRate`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#ac06928c5bf18c5db7eb4661dd783ba0a)，[`qualityAdaptIndication`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#a902fc5b33956471b7a28f43399fa8b20) 三个参数，分别反映目标码率、目标帧率与和上次返回的本地视频流统计信息相比，本地视频质量的自适应情况。

##### 2. 视频偏好设置

一般场景下，Agora 默认的视频编码配置能满足需求。对于特定场景，该版本提供如下功能让用户选择视频偏好：

- 弱网下画质或流畅偏好设置。该版本在视频编码属性 [`VideoEncoderConfiguration`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) 类中新增 2 个参数 minFrameRate 和 degradationPrefer，分别用于设置最低视频编码帧率，以及带宽受限时编码帧率的偏好。这两个参数需要搭配使用。
- 采集时预览或性能偏好设置。该版本新增接口 [`setCameraCapturerConfiguration`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af)，通过设置摄像头采集偏好，用户可以根据实际场景选择优先保证设备性能还是视频质量。具体场景及参数选择，请参考 [API 文档](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af)。

##### 3. 核心质量改进

- 降低了音频延时
- 提升了视频质量和稳定性
- 缩短了远端视频的出图时间
- 改善了屏幕共享直播弱⽹下视频流畅性和延迟

#### 问题修复

##### 音频

- 修复了调用 `enableLocalAudio` 接口导致的蓝牙断开的问题
- 新增支持中文字符音乐
- 修复了高音声音变弱的问题
- 修复了偶现的声音快放问题

##### 视频

- 通过增加 `renderMode` 的默认值，修复了用户在没有设置的情况 下，窗口和画面比例不符合引发的拉伸问题
- 部分低性能设备上出现的播放视频卡住的问题
- 优化了 SDK 出图时间

##### 其他

- 修复了偶现的崩溃问题
- 修复了降低设备分辨率时，屏幕共享流被裁剪的问题。我们建议你使用 v2.4.0 最新的屏幕共享接口解决该问题，新接口为屏幕共享采用独立的采集参数，不再与摄像头输入共用。详见[进行屏幕共享](../../cn/Video/screensharing_windows.md)。
- 修复了屏幕共享坐标为负时，无法共享桌面的问题
- 修复了偶现的的回声问题
- 修复了转码推流场景下，SEI 信息与媒体流不同步的问题

#### API 变更

为提升用户体验，Agora 在 v2.4.0 版本中对 API 进行了如下变动：

##### 新增

- [`setBeautyEffectOptions`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5899cc462e5250028c9afada4df98d48)
- [`startScreenCaptureByScreenRect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a41893fe9a0ca49c054bf6dbd7d9d68f5)
- [`startScreenCaptureByWindowId`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52)
- [`updateScreenCaptureParameters`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ad680e114ba3b8a0012454af6867c7498)
- [`setScreenCaptureContentHint`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aff9003c492450dbd8c3f3b9835186c95)
- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a70175273dade14bcf8d74e71f8de7eda)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#add46038703f2f82dad83c0319d43433b)
- [`enableSoundPositionIndication`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d)
- [`setRemoteVoicePosition`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af85b6be0a185fc67acc6eabe31467d20)
- [`startLastmileProbeTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adb3ab7a20afca02f5a5ab6fafe026f2b)
- [`stopLastmileProbeTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a94f3494035429684a750e1dee7ef1593)
- [`setRemoteUserPriority`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aecb5d85e9b3a60947d569b88253da710)
- [`startEchoTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a842ed126b6e21a39059adaa4caa6d021)
- [`startAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac78c08f3212dc3efa000e197207dec53)
- [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aad01da1e0bacd3f2fd355483f9e3befb)
- [`setCameraCapturerConfiguration`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af)
- [`setLogFileSize`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a6fb256cb165856a4412bb30b098408a1)
- [`onAudioMixingStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a298389513bfaa50af4277fc3296e3f22)
- [`onLastmileProbeResult`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a44134dfda5d412831fa8e44fa533fca5) 

##### 废弃

- `startEchoTest`
- `startScreenCapture`
- `setVideoQualityParameters`

**2.3.3 版**

该版本于 2019 年 1 月 24 日发布。功能改进与修复问题详见下文。

#### 改进

为提升用户体验，屏幕共享做了大量算法优化。针对不同的共享场景提供不同的屏幕共享策略，尤其针对 PPT 翻页和网页浏览场景，提升了屏幕共享过程中的流畅度和清晰度。同时改善了通信场景下屏幕共享开启阶段画面模糊的现象。

#### 问题修复

修复了 `onNetworkQuality` 回调不准确的问题。


**2.3.2 版**

该版本于 2019 年 1 月 16 日发布。新增特性与修复问题详见下文。

#### 升级必看

2.3.2 除了下文提到的功能和改进外，整体提升直播场景下视频弱网下抗丢包能力，提高流畅度，降低卡顿率。升级前，请了解版本兼容性:

- Native SDK 版本号须大于等于 1.11 版本
- Web SDK 版本号须大于等于 2.1 版本

如果你的 Native SDK 是由 v2.0.8 升级至 v2.3.2，Agora 建议你另外参考[升级指南](../../cn/Video/migration_windows.md)了解主要 API 的变动。

#### 新增功能

##### 1. 视频自采集（Push 模式）

为方便用户在通话或直播中使用外部视频数据，该版本新增如下接口，支持使用 Push 方式进行视频自采集。启用后，应用程序需要将外部的视频帧封装成  `AgoraVideoFrame` 格式后，推送给 Agora SDK 进行编码和传输。该方法适用于用户在发送端自己做采集、渲染，然后把视频帧发送给 Agora SDK 进行编码和传输的场景。本地渲染需要用户自己完成。

- [`setExternalVideoSource`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)：配置外部视频源
- [`pushVideoFrame`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)：推送外部视频帧

##### 2. 音频自渲染（Pull 模式）

为提升外部音频源的使用体验，该版本新增如下接口，支持使用 Pull 方式进行音频自渲染。启用后，应用程序会采用主动拉取的方式从音频引擎拉取远端已解码混音后的音频帧，用于外部音频播放。

- [`setExternalAudioSink`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638)： 设置外部音频自渲染
- [`pullAudioFrame`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940)：拉取音频帧用于外部播放

和语音观测器中的 [`onPlaybackAudioFrame`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac) 方法相比，该方法的优势在于：

- 使用 `onPlaybackAudioFrame` 方法时，SDK 主动每 10 ms 回调数据给应用程序。如果应用程序处理有延时，就有可能会导致音频播放抖动；
- 使用 `pullAudioFrame` 方法时，SDK 主动拉取音频帧。通过设置 **AudioFrame** 中的 `samples` 参数，SDK 可以指定需要的播放采样数量；同时 SDK 内部会通过调整缓存，帮助应用程序处理延时，从而有效避免因外部音频播放抖动导致的异常，如音频不同步等问题。Agora 推荐使用该方法。

##### 3. 提升直播清晰度 

Agora SDK 会根据网络条件进行码率自适应。为满足用户在直播场景下对视频清晰度的要求，该版本在 [setVideoEncoderConfiguration](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) 接口中新增 [minBitrate](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#ab3249ca88227aaf36f21681b9de7ced2) 参数，强制视频编码器输出高质量图片。如果将参数设为高于默认值，在网络状况不佳情况下可能会导致网络丢包，并影响视频播放的流畅度。因此如非对画质有特殊需求，Agora 建议不要修改该参数的值。

##### 4. 控制音乐文件的播放音量

为方便用户控制混音音乐文件的播放音量，该版本在已有 [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a5e117be71d38d813208198f4064aa964) 的基础上新增 [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085) 和 [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734) 接口，用于分别控制混音音乐文件在本地和远端的播放音量。

添加新的方法后，原有的 [adjustPlaybackSignalVolume](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a98919705c8b2346811f91f9ce5e97a79) 由控制人声和音乐的音量改为仅控制人声的音量。因此，如果要静音本地播放，需同时设置 `adjustPlaybackSignalVolume(0)` 和 `adjustAudioMixingPlayoutVolume(0)`。

该版本梳理了用户在音频采集到播放过程中可能会需要调整音量的场景，及各场景对应的 API，供用户参考使用。详见官网文档[调整通话音量](../../cn/Video/volume_windows.md)。

##### 5. 直播中弱网环境下视频自动回退/重开

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec) 和 [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7) 两个接口。 用户设置这两个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或直接关闭视频流，从而保证或提高音频质量。同时 SDK 会持续监控网络质量，并在网络质量改善时恢复音视频流。在推流回退为音频流时，或由音频流恢复为音视频流，触发 [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513) 或 [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74) 回调。

##### 6. 按用户返回音视频上下行码率、帧率、丢包率及延迟

为方便统计每个用户的音视频上下行码率、帧率及丢包率，该版本新增 [`onRemoteAudioTransportStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d) 和 [`onRemoteVideoTransportStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f) 回调。 通话或直播过程中，当用户收到远端用户发送的音视频数据包后，会周期性地发生该回调上报，频率约为 2 秒 1 次。 回调中包含用户的 UID、音/视频接收码率、丢包率、以及延迟时间（毫秒）。 并在统计频道内通话相关数据的 Rtcstats 类中增加 [`lastmileDelay`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a255549c958a1dcb0aad1eb0f13ff7f18) 参数，返回客户端到 vos 服务器的延迟。

##### 7. 设置视频编码属性

为满足场景中视频旋转的需要，提升自定义视频源画质，该版本引入 [`setVideoEncoderConfiguration`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) 替换原 [`setVideoProfile`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb) 接口，来设置视频编码属性。 新接口中的 [`VideoEncoderConfiguration`](https://docs.agora.io/cn/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) 类对应一套视频参数，包含视频的分辨率 `dimension`、帧率 `frame rate`、码率 `bitrate`、最低编码码率`minBitrate` 以及视频方向 `orientationMode`，其中 Agora 建议保留 `minBitrate` 的默认设置。原接口 `setVideoProfile` 仍可使用，但不再推荐。

##### 8. 虚拟声卡采集

为提升声卡采集易用性，该版本在 [`enableLoopbackRecording`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a065f485fd23b8c24a593680a47d754aa) 接口中新增参数 deviceName，支持用户使用虚拟声卡进行采集。该参数 NULL 时默认使用当前声卡采集。如需使用虚拟声卡，直接使用虚拟声卡的产品名传参即可。


#### 改进

##### 1. 提供更透明的质量数据统计

为提升质量透明的用户体验，该版本废弃了原有的 `onAudioQuality` 回调，并新增 `onRemoteAudioStats` 回调进行取代。和原来的接口相比，新接口使用更为综合的算法，通过引入音频丢帧率、端到端的音频延迟、接收端网络抖动的缓冲延迟等参数，使回调结果更贴近用户感受。同时，该版本优化了 `onNetworkQuality` 的算法，对上下行网络质量采用不同的计算方法，使评分更精准。

- [`onRemoteAudioStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)：通话中远端音频流的统计信息回调。用于替换	`onAudioQuality`
- [`onNetworkQuality`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80003ae8cce02039f3aa0e8ffad7deed)：通话中每个用户的网络上下行 Last mile 质量报告回调。

Agora SDK 计划在下一个版本对如下 API 进行进一步改进：

- [`onLastmileQuality`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33)：通话前网络上下行 Last mile 质量报告回调

该版本对数据统计相关回调进行了统一梳理，相关回调及算法详见[通话中数据统计](../../cn/Video/in-call_quality_windows.md)。

##### 2. 改进获取 SDK 网络连接状态的生成策略

为提升 SDK 使用数据统计的准确性和合理性，该版本新增如下接口，用以获取 SDK 的网络连接状态，以及连接状态发生改变的原因。

- [`getConnectionState`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362)：获取 SDK 的网络连接状态
- [`onConnectionStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)：SDK 的网络连接状态已改变回调

该版本废弃了原有的 [`onConnectionInterrupted`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483) 和 [`onConnectionBanned`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404) 回调。

在新的接口下，SDK 共有 5 种连接状态：未连接、正在连接、已连接、正在重新建立连接和连接失败。当连接状态发生改变时，都会触发 `onConnectionStateChanged` 回调。当条件满足时，原有的 `onConnectionInterrupted` 和 `onConnectionBanned` 回调也会触发，但 Agora 不再推荐使用。


##### 3. 优化打分反馈机制

为方便用户（开发者）收集最终用户（应用程序使用者）对使用应用进行通话或直播的反馈，该版本将 [`rate`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a748c30a6339ec9798daa0d1b21585411) 接口中的打分范围缩减为 1 - 5，减少最终用户的打分干扰。Agora 建议在应用程序中集成该接口，方便应用程序收集用户反馈。


##### 4. 音频设备热插拔逻辑优化，提升音频可用性

优化了通话中和直播中的设备插拔选择，避免因用户主动切换设备而导致音频无声。

##### 5. 其他优化

- 优化了直播场景下视频弱网抗丢包能力
- 加快了严重拥塞状态视频的恢复速度
- 提升了推流稳定性
- 优化了 API 的调用线程
- 增加了重新检测耳机插入和蓝牙设备连接的代码
- 降低了音频延时
- 优化了屏幕共享的性能

#### 问题修复

##### SDK 相关

修复了检测设备时出现的闪退问题

##### 音频

- 修复了设备在连接蓝牙的状态下，退出频道后，音频不走蓝牙的问题
- 修复了调用 `startAudioMixing` 播放音乐文件时出现的崩溃问题
- 修复了麦克风在禁用的状态下，设备插上耳机后，禁用失效的问题
- 修复了外放条件下，上下麦、系统电话打断、进退频道等多种场景下，出现的无法调节外放音量的问题
- 修复了应用从后台切回前台时，出现的出声音慢的问题
- 修复了音频模块的崩溃问题

##### 视频

- 修复了因视频编码问题引起的 Native 设备与 Web 端互通时，Web 端看不到 Native 端视频画面的问题
- 修复了 x86 设备上自采集图像时硬件编码器的相关问题
- 修复了视频自采集时的偶现问题
- 修复了 Windows 设备上出现的屏幕共享时鼠标位置问题
- 修复了首帧出图时间过长的问题
- 修复了共享桌面指定区域过程中，调用 `setVideoProfile` 设备视频编码属性时，出现的共享画面左右颠倒的问题
- 修复了屏幕共享时出现的本地和远端鼠标位置不一致的问题
- 修复了使用高分辨率视频屏幕共享时出现的卡糊的问题


#### API 变更

为提升用户体验，Agora 在 v2.3.2 版本中对 API 进行了如下变动：

##### 新增

- [`setVideoEncoderConfiguration`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e)
- [`setExternalVideoSource`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)
- [`pushVideoFrame`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)
- [`setExternalAudioSink`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638)
- [`pullAudioFrame`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7)
- [`getConnectionState`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734)
- [`onConnectionStateChanged`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)
- [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513)
- [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74)
- [`onRemoteAudioStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)
- [`onRemoteAudioTransportStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d)
- [`onRemoteVideoTransportStats`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f)

##### 废弃

- [`setVideoProfile`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb)
- [`onAudioQuality`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a36ad42975f3545382de07875016fb7fa)
- [`onConnectionInterrupted`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483)
- [`onConnectionBanned`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404)

**2.2.2 版**

该版本于 2018 年 6 月 21 日发布。修复问题列表详见下文。

#### 修复问题

- 修复了偶发的线上统计崩溃的问题
- 修复了特定场景下偶发的视频窗口尺寸变化后，视频卡住的问题
- 修复了偶发的无法正常反馈频道内谁在说话以及说话者的音量的问题

**2.2.1 版**

该版本于 2018 年 5 月 30 日发布。内部代码优化。

**2.2.0 版**

该版本于 2018 年 5 月 4 日发布。新增特性与修复问题列表详见下文。

#### 升级必看

为更好地提升用户体验，Agora SDK 在 2.1 版本中对动态秘钥进行了升级。 如果你当前使用的 SDK 是 2.1 之前的版本，并希望升级到 2.1 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。

#### 新增功能

本次发版新增如下功能：

##### 1. 音效混响进频道

播放音效 `playEffect` 接口新增了一个 `publish` 参数，用于在播放音效时，远端用户可以听到本地播放的音效。

> 如果你的 SDK 是由之前版本升级到 v2.2 版本，请务必关注该接口功能的变动。

##### 2. 服务端部署代理服务器

通过 Agora 部署的代理服务器，方便有企业防火墙的用户设置代理服务器，以使用 Agora 的服务。

修复了连麦后退出频道后再进入频道连麦对端看不到自己的问题。

##### 3. 获取远端视频状态

新增 `onRemoteVideoStateChanged` 接口，以获知远端视频流的状态。

##### 4. 视频水印

在本地直播及旁路直播中增加水印功能，允许用户将一张 PNG 图片作为水印添加到正在进行的本地直播或旁路直播中。新增 `addVideoWatermark` 和 `clearVideoWatermarks` 接口，以添加或删除本地直播水印；`LiveTranscoding` 接口中新增 `watermark` 参数，用于控制旁路直播中水印的添加。

#### 改进功能

本次发版改进如下功能：

##### 1. 当前说话者音量提示

改进 `enableAudioVolumeIndication` 接口的功能，无论频道内是否有人说话，都会在回调中按设置的时间间隔返回说话者音量提示。

##### 2. 频道内网络质量监测

根据用户对频道内实时网络质量监测的需求，在 `onNetworkQuality` 中改进了返回数据的准确度。

##### 3. 进入频道前网络条件监测

为方便用户在进频道前检查当前网络是否能支撑语音或视频通话，在 `onLastmileQuality` 中，由通过恒定码率监测优化为根据用户设定的 Video Profile 的码率进行监测，提高返回数据的准确度。且在网络状态为 unknown 时，依然以 2 秒的间隔返回回调。

##### 4. 提升音乐场景下的音质

提升了用户在播放音乐等场景下的音乐音质。

#### 修复问题

- 修复了大量用户同时直播连麦时，偶发的抖屏现象

- 修复了 Windows 设备在直播场景下偶发的音频卡顿问题


**2.1.3 版**

该版本于 2018 年 4 月 19 日发布。新增特性与修复问题列表详见下文。

#### 升级必看

该版本的 SDK 修改了 `setVideoProfile` 方法在直播场景下的码率值，修改后的码率值与 2.0 版本一致。

#### 问题修复

修复了部分手机上，用户离开频道后，开启自带的录音设备时，偶现录音出错的问题。

#### 改进

改进了通信和直播场景下屏幕共享的效果，缩短了用户从屏幕共享模式切换回普通模式需要的时间间隔。

**2.1.2 版**

该版本于 2018 年 4 月 2 日发布。新增特性与修复问题列表详见下文。

#### 升级必看

SDK 升级至 2.1.2 的直播场景后，相同分辨率下，视频更清晰，但带宽也会变大。

#### 问题修复

- 修复了连麦后退出频道后再进入频道连麦对端看不到自己的问题。

- 修复了通信场景的屏幕共享清晰度小于直播场景的问题。


**2.1.1 版**

该版本于 2018 年 3 月 16 日发布。

请正在或已集成 2.1 SDK 的客户尽快升级更新！ 本次发版修复了一个的系统风险，请尽快升级以免对服务造成影响。

**2.1.0 版**

该版本于 2018 年 3 月 7 日发布。新增特性与修复问题列表详见下文。

#### 新增功能

本次发版新增如下功能：

##### 1. 开黑

新增了一个游戏开黑场景，用于节省流量和去除杂音，通过调用 API `setAudioProfile` 实现。

##### 2. 音效均衡和音效混响

在直播场景下，主播如果需要通过内置的麦克风美化和定制自己的语音输入，可以通过调用 API `setLocalVoiceEqualization `和 `setLocalVoiceReverb` 轻易地设置音效均衡和混响来实现所需要的效果。

##### 3. 在线频道信息查询

新增 RESTful API 查询用户在频道中的状态信息，查询指定频道内的分角色用户列表，查询厂商频道列表，查询用户是否为连麦用户等。

##### 4. 17 人视频

在直播场景下，同一频道内支持 17 位主播同时进行视频直播和连麦。


##### 5. 插入外部视频源

直播场景下，可以将采集到的视频添加到正在进行的直播中，直播室里的主播和观众可以一起边看电影、比赛或演出，边进行点评、互动等功能，会让现有的直播话题更广、体验更好。 仅支持拉入一路流，格式包括: RTMP, HLS, FLV。赛事直播最多同时支持 5 人连麦直播。详见 [外部输入直播视频源](../../cn/Video/inject_stream_windows.md) 。


##### 6. 新增直播场景下的屏幕共享功能

- 在 2.1.0 以前: Agora SDK 仅支持视频通话场景下的屏幕共享功能;

- 从 2.1.0 开始: Agora SDK 正式支持直播场景下的屏幕共享功能;


##### 7. 声卡采集

新增 API `enableLoopbackRecording` 开启视声卡采集，开启后 SDK 可以采集到本地播放的所有声音。

#### 改进

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



#### 问题修复

- 修复了摄像头采集失败问题;

- 修复了偶现的崩溃;

- 修复了偶现的视频卡顿问题;


**2.0 版**

该版本于 2017 年 12 月 6 日发布。新增特性与修复问题列表详见下文。

#### 升级必看

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
-   [录制 API](https://docs.agora.io/cn/Video/API%20Reference/recording_cpp/index.html)


方案二：如果你希望继续使用 Agora Recording Server，维持 Windows SDK 版本不变（<=v1.14），将不影响您继续使用 Windows SDK的 API 触发录制。

若您有任何疑问，可以通过以下方式获得技术支持:

1.  通过 Agora 开发者社区提问
2.  通过 Agora 工单系统提交工单
3.  您还可以随时联系技术支持群里的同事或商务


#### 新增功能

- 通信场景支持视频大小流功能，新增 API `setRemoteVideoStreamType` 和 `enableDualStreamMode` ;

- 通信和直播场景下支持音频自采集功能，新增以下 API:

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


- 通信和直播场景下支持服务端踢人功能。如有需要，请联系 [sales@agora.io](mailto:sales@agora.io) 开通该功能。

- 支持 Windows 64 位;

- 增加音量、静音设置/获取接口以及系统音量、静音同步引擎事件接口, 详见:

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


#### 问题修复

修复了加入频道后出现的崩溃。

**1.14 版**

该版本于 2017 年 10 月 20 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `setAudioProfile `设置音频参数和应用场景。

-   新增 API `setLocalVoicePitch` 提供基础变声功能。

-   直播场景: 新增 API `setInEarMonitoringVolume` 提供调节耳返音量功能。


#### 改进

-   优化了在高码率下的音频体验。

-   秒开: 直播场景下，单流模式时观众加入频道 1 秒内看见主播图像（均值为 885 ms, 网络状态良好时可达 788 ms）。

-   节省带宽:

    -   1.14 以前: 如果你选择不听某人的音频或不看某人的视频，音视频流会照发。

    -   1.14 开始: 如果你选择不听或不看某人的流，则不会下发，从而节省带宽。

-   精准的码率控制:

    -   1.14 以前: 码率控制不够精准，上下波动幅度较大。波动过大容易造成网络拥塞，增加丢包、丢帧的概率，影响了带宽估计模块的精度，特别是在弱网低码率情况下尤为明显。

    -   1.14 开始: 精准的码率控制，要多少给多少，不多给也不少给，避免波动过大造成的网络拥塞，减少传输延时，有助于减少网络卡顿。


#### 问题修复

-   修复了部分 Windows 机器上无声音的问题。

-   修复了部分 Windows 机器上的摄像头问题。


**1.13.1 版**

该版本于 2017 年 9 月 28 日发布。新增特性与修复问题列表详见下文。

#### **优化**

优化了特定场景下出现的回声问题。

**1.13 版**

该版本于 2017 年 9 月 4 日发布。新增特性与修复问题列表详见下文。

#### 新增功能

-   新增 API `onClientRoleChanged` 用于提醒直播场景下主播、观众上下麦的回调。

-   新增单独关闭语音播放的功能。

-   新增功能支持服务端推流失败回调。

-   屏幕共享直播场景下采集声卡选项动态开启和关闭。


#### 改进

-   软编情况下，视频属性可控。

-   可以在客户端设置推流的 Profile。

-   屏幕共享: 提升了画质清晰度和流畅度。

-   屏幕共享: 通信场景下新增动态更新截图区域。


#### 修复问题

修复了部分机型上偶现的崩溃。

**1.12 版**

该版本于 2017 年 7 月 25 日发布。新增特性与修复问题列表详见下文。

#### 新增功能

-   直播场景下， 新增 API 方法 `injectStream` 在当前频道内插入一条 RTMP 流。该功能目前为 beta 版

-   在 API 方法 `setEncryptionMode` 里新增加密模式 `aes-128-ecb` 。

-   在 API 方法 `startAudioRecording` 里新增参数 `quality`用于设置录音音质。

-   新增 API `onActiveSpeaker` 提示当前频道内谁在说话。

-   通信场景下，删除了原有的 API 方法 `setScreenCaptureWindow`，更新 API 方法 `startScreenCapture` 共享整个屏幕、指定窗口或指定区域

-   通信场景下，启用屏幕共享功能后，在屏幕共享过程中可以显示鼠标


#### 改进

通信场景下针对 320 x 180 分辨率提供了以下改进方案:

-   网络和设备状态较差的情况下仍能保证画质流畅度。

-   网络和设备状态良好的情况下可以做到比 180P 更好的画质清晰度。


#### 修复问题

修复了部分机型上偶现的崩溃问题。

