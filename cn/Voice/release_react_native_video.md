
---
title: 发版说明
description: Release notes
platform: React Native
updatedAt: Mon Sep 28 2020 13:16:11 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora React Native SDK 的发版说明。

## 简介

Agora React Native SDK 基于 Android 和 iOS 平台的 Agora RTC SDK 封装，可在基于 React Native 开发的 Android 和 iOS 移动端应用中快速实现实时音视频功能。

Agora React Native SDK 主要支持两种场景：

- 音视频通话
- 音视频直播

点击[语音通话产品概述](https://docs.agora.io/cn/Video/product_voice)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video)、[音频互动直播产品概述](https://docs.agora.io/cn/Video/product_live_audio)及[视频互动直播](https://docs.agora.io/cn/Video/product_live)了解关键特性。


## 3.1.2 版

该版本于 2020 年 9 月 28 日发布。

**升级必看**

#### 1. 关闭本地网络连通质量报告功能 （仅 iOS）

为提升用户体验，防止 app 在 iOS 14.0 版本上触发查看本地网络设备的弹窗提示，该版本默认关闭本地网络连通质量报告功能。[`RtcStats`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcstats.html) 中的 [`gatewayRtt`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcstats.html#gatewayrtt) 参数会失效，请不要使用该参数获取客户端到本地路由器的往返延时。详见 [FAQ](https://docs.agora.io/cn/faq/local_network_privacy)。 如果你不介意弹窗提示，且需启用本地网络连通质量报告功能，请[提交工单](https://agora-ticket.agora.io/)联系声网技术支持。

#### 2. 指定访问区域完善

该版本修改了区域访问限制的区域码 `AreaCode`，最新区域码如下：

- `CN`：中国大陆。
- `NA`：北美区域。
- `EU`：欧洲区域。
- `AS`：除中国大陆以外的亚洲区域。
- `JP`：日本。
- `IN`：印度。
- `GLOB`：（默认）全球。

如你此前调用 [`createWithAreaCode`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/classes/rtcengine.html#createwithareacode) 方法时指定了访问区域，在由之前版本升级至该版本时，请确保使用正确的区域码。

**新增特性**

#### 1. 发布和订阅状态转换回调

该版本新增以下回调方便你了解音视频流当前的发布及订阅状态，有助于订阅和发布相关的数据统计：

- `AudioPublishStateChanged`：音频发布状态发生改变。
- `VideoPublishStateChanged`：视频发布状态发生改变。
- `AudioSubscribeStateChanged`：音频订阅状态发生改变。
- `VideoSubscribeStateChanged`：视频订阅状态发生改变

#### 2. 本地首帧发布回调

为提示用户本地音视频首帧已发布，该版本新增如下回调：

- `FirstLocalAudioFramePublished`：已发布本地音频首帧回调。该回调取代 `FirstLocalAudioFrame` 回调，我们建议你不再使用 `FirstLocalAudioFrame` 回调。
- `FirstLocalVideoFramePublished`：已发布本地视频首帧回调。

**改进**

#### 1. CDN 直播推流

该版本新增 `RtmpStreamingEvent` 回调，报告 CDN 直播推流过程中发生的事件，如未成功添加背景图或水印。

#### 2. 加密

该版本新增 `enableEncryption` 方法，用于开启内置加密，并废弃原加密方法：

- `setEncryptionSecret`
- `setEncryptionMode`

与原加密方法相比，该方法新增对国密 SM4 加密模式的支持，你可以根据需要选择合适的加密模式。

#### 3. 通话中质量透明

该版本进一步扩充了 `LocalAudioStats` 类、`LocalVideoStats` 类、`RemoteVideoStats` 类和 `RemoteAudioStats` 类的成员，提供更多音视频质量相关数据。

- `LocalAudioStats` 类新增 `txPacketLossRate`，表示本端到 Agora 边缘服务器的物理音频丢包率 (%)。

- `LocalVideoStats` 类新增：

  - `txPacketLossRate`：本端到 Agora 边缘服务器的物理视频丢包率 (%)。
  - `captureFrameRate`：本地视频采集帧率 (fps)。

- `RemoteAudioStats` 和 `RemoteVideoStats` 类中新增 `publishDuration`，表示远端音频流和视频流的累计发布时长（毫秒）。

#### 4. 设置音频编码属性

为提升音频性能，该版本对音频编码码率最大值进行如下优化：

| Profile                  | 3.1.2 版本                         | 3.1.2 版本之前                     |
| :----------------------- | :--------------------------------- | :--------------------------------- |
| `Default`                | 直播场景: 64 Kbps</br>通信场景: 18 Kbps | 直播场景: 52 Kbps</br>通信场景: 18 Kbps |
| `SpeechStandard`         | 18 Kbps                            | 18 Kbps                            |
| `MusicStandard`          | 64 Kbps                            | 48 Kbps                            |
| `MusicStandardStereo`    | 80 Kbps                            | 56 Kbps                            |
| `MusicHighQuality`       | 96 Kbps                            | 128 Kbps                           |
| `MusicHighQualityStereo` | 128 Kbps                           | 192 Kbps                           |

#### 5. 日志扩容

该版本中，Agora SDK 日志文件的默认个数由 2 个增加至 5 个，单个日志文件的默认大小由 512 KB 扩大至 1024 KB。默认情况下，SDK 会生成 `agorasdk.log`、`agorasdk_1.log`、`agorasdk_2.log`、`agorasdk_3.log`、`agorasdk_4.log` 这 5 个日志文件。最新的日志永远写在 `agorasdk.log` 中。`agorasdk.log` 写满后，SDK 会从 1-4 中删除修改时间最早的一个文件，然后将 `agorasdk.log` 重命名为该文件，并建立新的 `agorasdk.log` 写入最新的日志。

#### 6. TextureView 实现方式

该版本改进了 `TextureView` 的底层实现方式，从实例化 `TextureView` 对象改为用 `CreateTextureView` 方法创建 `TextureView` 对象。

#### 7. 其他改进

- 降低了音频延时。
- 降低了远端用户音频首帧解码时间。
- 该版本对 iOS 部分机型（使用 Apple A10 及以下芯片）进行性能优化，降低了 CPU 使用率和内存占用。
- 该版本在 OPPO 如下机型上降低了耳返延时：
  - Reno4 Pro 5G
  - Reno4 5G

**问题修复**

该版本修复了如下问题：

- `FirstLocalVideoFrame` 和 `FirstRemoteVideoFrame` 回调的触发时机不准确。
- 调用 `setAudioMixingPitch` 方法时，部分参数不生效。
- 美颜功能不生效。
- Android 平台：
  - 特定场景下视频画面模糊。
  - Android 6 及以上且 CPU 为 x86 架构的手机出现崩溃。
- iOS 平台：
  - 远端用户退出频道时，本地用户看远端视频黑屏。
  - 断开蓝牙设备时，偶现音频卡顿。
  - 多频道场景中，用户切换频道发言后，退出频道时偶现崩溃。

**API 变更**

#### 新增

- [`AudioPublishStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#audiopublishstatechanged)
- [`VideoPublishStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#videopublishstatechanged)
- [`AudioSubscribeStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#audiosubscribestatechanged)
- [`VideoSubscribeStateChanged`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#videosubscribestatechanged)
- [`FirstLocalAudioFramePublished`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#firstlocalaudioframepublished)
- [`FirstLocalVideoFramePublished`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#firstlocalvideoframepublished)
- [`enableEncryption`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/classes/rtcengine.html#enableencryption)
- [`LocalAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/localaudiostats.html) 类中新增 [`txPacketLossRate`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/localaudiostats.html#txpacketlossrate)
- [`LocalVideoStats`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/localvideostats.html) 类中新增 [`txPacketLossRate`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/localvideostats.html#txpacketlossrate) 和 [`captureFrameRate`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/localvideostats.html#captureframerate)
- [`RemoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/remoteaudiostats.html) 和 [`RemoteVideoStats` ](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/remotevideostats.html)类中新增 `publishDuration`
- [`RtmpStreamingEvent`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#rtmpstreamingevent)
- 错误码：[`NoServerResources(103)`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/enums/errorcode.html#noserverresources)
- 警告码：
  - [`AdmCategoryNotPlayAndRecord(1029)`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/enums/warningcode.html#admcategorynotplayandrecord)
  - [`AdmNoDataReadyCallback(1040)`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/enums/warningcode.html#admnodatareadycallback)
  - [`AdmInconsistentDevices(1042)`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/enums/warningcode.html#adminconsistentdevices)
  - [`ApmResidualEcho(1053)`](https://docs.agora.io/cn/Voice/API%20Reference/react_native/enums/warningcode.html#apmresidualecho)

#### 废弃

- `setEncryptionSecret`
- `setEncryptionMode`
- `FirstLocalAudioFrame`

#### 删除

- 警告码：`AdmImproperSettings(1053)`
- 
## 3.0.1 版

该版本于 2020 年 9 月 4 日发布。功能特性及相关文档详见下文。

**功能特性**

1. 全面适配 Agora RTC SDK v3.0.1。
2. 支持多频道管理。
3. 全面支持异步编程 （Promises）。
4. Android 平台支持 `TextureView` 渲染。

**相关文档**

你可以参考以下文档集成 SDK，实现相应的实时音视频功能：

- 快速开始：
 - [实现语音通话](../../cn/Voice/start_call_audio_react_native.md)
 - [实现视频通话](../../cn/Voice/start_call_react_native.md)
 - [实现视频互动直播](../../cn/Voice/start_live_react_native.md)
 - [实现音频互动直播](../../cn/Voice/start_live_audio_react_native.md)
- [API 参考](https://docs.agora.io/cn/Voice/API%20Reference/react_native/index.html)

Agora 在 GitHub 提供一个开源的 [Agora React Native Quickstart](https://github.com/AgoraIO-Community/Agora-RN-Quickstart) 示例项目。你也可以前往下载并体验。
