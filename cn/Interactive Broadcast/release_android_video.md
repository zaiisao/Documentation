
---
title: 发版说明
description: 
platform: Android
updatedAt: Tue Mar 19 2019 02:59:11 GMT+0000 (UTC)
---
# 发版说明
本文提供 Agora 视频 SDK 的发版说明。

## **简介**

Android 视频 SDK 支持两种主要场景:

-   音视频通话
-   音视频直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video?platform=All%20Platforms)以及 [互动直播产品概述](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms) 了解关键特性。

### 已知问题和局限性

#### 隐私权变更

如果您的应用以 Android 9 为目标平台，您应牢记以下行为变更。对设备序列信息和 DNS 信息进行的这些更新可增强用户隐私保护。

**构建序列号弃用**

在 Android 9 中，`Build.SERIAL` 始终设置为 "UNKNOWN" 以保护用户的隐私。
如果您的应用需要访问设备的硬件序列号，您应改为请求 READ_PHONE_STATE 权限，然后调用 `getSerial()`。

**DNS 隐私**

以 Android 9 为目标平台的应用应采用私有 DNS API。 具体而言，当系统解析程序正在执行 DNS-over-TLS 时，应用应确保任何内置 DNS 客户端均使用加密的 DNS 查找与系统相同的主机名，或停用它而改用系统解析程序。

详情请参考 [Android 隐私权变更](https://developer.android.com/about/versions/pie/android-9.0-changes-28?hl=zh-CN#privacy-changes-p)。

## **2.3.3 版**

该版本于 2019 年 1 月 24 日发布。修复问题详见下文。

### **问题修复**

- 修复了 `onNetworkQuality` 回调不准确的问题。
- 修复了特定场景下华为 P9 上偶发的崩溃问题。

## **2.3.2 版**

该版本于 2019 年 1 月 16 日发布。新增特性与修复问题列表详见下文。

### **升级前必看**

2.3.2 整体提升直播模式下视频弱网抗丢包能力，提高流畅度，降低卡顿率。升级前，请了解以下版本兼容性:

* Native SDK 版本不能低于 1.11.0
* Web SDK（若互通）版本不能低于 2.1.0

### **新增功能**

#### 1. 对兴趣点自动曝光

为提升视频采集质量，该版本新增如下接口，支持对兴趣点自动曝光功能。开发者可以将需要自动曝光的区域位置发送给 Agora SDK，摄像头会基于该区域自动曝光。

- [`isCameraExposurePositionSupported`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6818c2a98bebeb72e4802b1c585da99b)：检查设备是否支持摄像头曝光
- [`setCameraExposurePosition`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0ac20919f60df42635850c53c9cbdefd)：设置摄像头曝光区域
- [`onCameraExposureAreaChanged`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ab6bc82a55191e596d5bf5a7c56bdf95e)：摄像头曝光区域已更改

#### 2. 提升直播清晰度

Agora SDK 会根据网络条件进行码率自适应。为满足用户在直播场景下对视频清晰度的要求，该版本在 [`setVideoEncoderConfiguration`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af5f4de754e2c1f493096641c5c5c1d8f) 接口中新增 [`minBitrate`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1video_1_1_video_encoder_configuration.html#a9cd44566bc19eca4006fda264ea96dc7) 参数，强制视频编码器输出高质量图片。如果将参数设为高于默认值，在网络状况不佳情况下可能会导致网络丢包，并影响视频播放的流畅度。因此如非对画质有特殊需求，Agora 建议不要修改该参数的值。

#### 3. 控制音乐文件的播放音量 

为方便用户控制混音音乐文件在本地及远端的播放音量，该版本在已有 [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a) 的基础上新增 [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9) 和 [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00) 接口，用于分别控制混音音乐文件在本地和远端的播放音量。

该版本梳理了用户在音频采集到播放过程中可能会需要调整音量的场景，及各场景对应的 API，供用户参考使用。详见官网文档[调整通话音量](../../cn/Interactive%20Broadcast/volume_android.md)。


### **改进**

#### 1. 提供更透明的质量数据统计

为提升质量透明的用户体验，该版本废弃了原有的 `onAudioQuality` 回调，并新增 `onRemoteAudioStats` 回调进行取代。和原来的接口相比，新接口使用更为综合的算法，通过引入音频丢帧率、端到端的音频延迟、接收端网络抖动的缓冲延迟等参数，使回调结果更贴近用户感受。同时，该版本优化了 `onNetworkQuality` 的算法，对上下行网络质量采用不同的计算方法，使评分更精准。

- [`onRemoteAudioStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54)：通话中远端音频流的统计信息回调。用于替换	`onAudioQuality`
- [`onNetworkQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a76be982389183c5fe3f6e4b03eaa3bd4)：通话中每个用户的网络上下行 Last mile 质量报告回调

Agora SDK 计划在下一个版本对如下 API 进行进一步改进：

- [`onLastmileQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5)：通话前网络上下行 Last mile 质量报告回调

该版本对数据统计相关回调进行了统一梳理，相关回调及算法详见[通话中数据统计](../../cn/Interactive%20Broadcast/in_call_statistics_android.md)。

#### 2. 改进获取 SDK 网络连接状态的生成策略

为提升 SDK 使用数据统计的准确性和合理性，该版本新增如下接口，用以获取 SDK 的网络连接状态，以及连接状态发生改变的原因。

- [`getConnectionState`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8635e3c9e26ffe95e7ab9a518af533b9) ：获取 SDK 的网络连接状态
- [`onConnectionStateChanged`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e)：SDK 的网络连接状态已改变回调

该版本废弃了原有的 [`onConnectionInterrupted`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0841fb3453a1a271249587fa3d3b3c88) 和 [`onConnectionBanned`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80cfde2c8b1b9ae499f6d7a91481c5db) 回调。

在新的接口下，SDK 共有 5 种连接状态：未连接、正在连接、已连接、正在重新建立连接和连接失败。当连接状态发生改变时，都会触发 `onConnectionStateChanged` 回调。当条件满足时，原有的 `onConnectionInterrupted` 和 `onConnectionBanned` 回调也会触发，但 Agora 不再推荐使用。


#### 3. 优化打分反馈机制

为方便用户（开发者）收集最终用户（应用程序使用者）对使用应用进行通话或直播的反馈，该版本将 [`rate`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab7083355af531cc43d455024bd1f7662) 接口中的打分范围缩小为 1 - 5，减少最终用户的打分干扰。Agora 建议在应用程序中集成该接口，方便应用程序收集用户反馈。

#### 4. 其他改进

- 优化了直播模式下视频弱网抗丢包能力
- 加快了严重拥塞状态视频的恢复速度
- 提升了推流稳定性
- 优化了 SDK 在 Android 6.0 以上设备上的性能
- 优化了 API 的调用线程
- 增加了重新检测耳机插入和蓝牙设备连接的代码
- 降低了音频延时
- 新增了对锤子手机摄像头的适配


### **问题修复**

#### SDK 相关：

- 修复了 SDK 在部分模拟器（夜神、mumu 等）上的崩溃问题
- 修复了 Android 6.0 以上 x86 so 重定位导致的崩溃问题

#### 音频相关：

- 修复了设备在连接蓝牙的状态下，退出频道后，音频不走蓝牙的问题
- 修复了调用 `startAudioMixing` 播放音乐文件时出现的崩溃问题
- 修复了麦克风在禁用的状态下，设备插上耳机后，禁用失效的问题
- 修复了华为 Mate 20X 上出现的应用切至后台，使用其他应用时出现的远端听不到本地说话的问题
- 修复了 Pixel 2 设备上出现的回声问题
- 修复了外放条件下，上下麦、系统电话打断、进退频道等多种场景下，出现的无法调节外放音量的问题
- 修复了应用从后台切回前台时，出现的出声音慢的问题

#### 视频相关：

- 修复了 x86 设备上自采集图像时硬件编码器的相关问题
- 修复了视频自采集时的偶现问题


### **API 整理**

为提升用户体验，Agora 在 v2.3.2 版本中对 API 进行了如下变动。

#### 新增

- [`isCameraExposurePositionSupported`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6818c2a98bebeb72e4802b1c585da99b)
- [`setCameraExposurePosition`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0ac20919f60df42635850c53c9cbdefd)
- [`getConnectionState`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8635e3c9e26ffe95e7ab9a518af533b9)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00)
- [`onConnectionStateChanged`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e)
- [`onCameraExposureAreaChanged`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ab6bc82a55191e596d5bf5a7c56bdf95e)
- [`onRemoteAudioStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54)

#### 废弃

- [`onAudioQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#abeac442a777e103536dcf9c8ce2d7135)
- [`onConnectionInterrupted`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0841fb3453a1a271249587fa3d3b3c88)
- [`onConnectionBanned`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80cfde2c8b1b9ae499f6d7a91481c5db)

## **2.3.1 版**

该版本于 2018 年 10 月 12 日发布。新增特性与修复问题列表详见下文。

### **新增功能**

####  关闭/重新开启本地语音功能

应用程序在加入频道时，语音功能是默认打开的。为满足用户只接收而不发送音频流的需求，该版本新增 `enableLocalAudio` 接口，方便应用程序在进入频道后关闭或重新开启本地语音功能。关闭本地语音功能后，应用程序会收到 `onMicrophoneEnabled` 回调，并停止采集本地音频流。该方法不影响接收和播放远端音频流。

该功能与 `muteLocalAudioStream` 的区别在于前者不采集不发送，而后者是采集但不发送。

### **改进**

* 提升了 SDK 与 Android 设备上视频编码器的适配度。

### **问题修复**

* 修复了直播模式下，多次上下麦后某些 Android 设备上偶现的崩溃问题。
* 修复了切换前后摄像头过程中偶现的崩溃问题。
* 修复了通信模式下，一对一通话过程中，一端关闭视频后再打开，另一端出图较慢的问题。
* 修复了直播模式下，观众端因统计有误出现的延迟的问题。
* 修复了某些 Android 设备上偶现的无法渲染视频的问题。
* 修复了某些 Android 设备上偶现的视频卡顿的问题。
* 修复了某些 Android 设备上偶现的退出频道时，如果频道内还有人在说话，会听到声音且声音破音的问题。
* 修复了采集到的视频裸数据中，视频帧的时间戳不按帧更新的问题。

## **2.3.0 版**

该版本于 2018 年 8 月 31 日发布。新增特性与修复问题列表详见下文。

Agora SDK 在 v2.3.0 版本中，全面提升了视频功能的稳定性及可用性，保证了更加可靠的实时通信。同时音视频质量也得到进一步提高。 视频方面，通过优化编码性能，增强了弱网对抗能力，减少卡顿时间，提升视频流畅度；音频方面，采用深度学习算法，改进了通话中的音频质量。

### **升级必看**

-   为满足场景中视频旋转的需要，提升自定义视频源画质，该版本引入 `setVideoEncoderConfiguration` 替换原 `setVideoProfile` 接口。 `setVideoProfile` 接口仍可用，但不再推荐。

    -   直播模式下支持 Adaptive Mode，当发送端画面旋转时不再剪切画面，避免播放端画面出现“大头”或缩放模糊的现象。

    -   自采集场景中，可以根据输入视频帧的宽和高，动态调整输出视频帧的宽和高，尽可能避免剪切，并提供更多的图像信息给到播放端。

-   该版本中 LiveTranscoding Class 从 `io.agora.live `Package 移到了 `io.agora.rtc.live` Package。

-   该版本修改了 API `constants.java` 中的拼写错误。

    -   修改前：

    ```
    public static final int SOFEWARE_ENCODER = 1;
    ```

    -   修改后：

    ```
    public static final int SOFTWARE_ENCODER = 1;
    ```

-   为更好地提升用户体验，Agora SDK 在 v2.1.0 版本中对动态秘钥进行了升级。如果你当前使用的 SDK 是 v2.1.0 之前的版本，并希望升级到 v2.1.0 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。


### **新增功能**

本次发版新增如下功能：

#### 1. 直播中弱网环境下视频自动回退/重开

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 `setLocalPublishFallbackOption` 和 `setRemoteSubscribeFallbackOption` 两个接口。 用户设置这两个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或直接关闭视频流，从而保证或提高音频质量。同时 SDK 会持续监控网络质量， 并在网络质量改善时恢复音视频流。在推流回退为音频流时，或由音频流恢复为音视频流，触发 `onLocalPublishFallbackToAudioOnly` 或 `onRemoteSubscribeFallbackToAudioOnly` 回调。

#### 2. 提前 30 秒提醒 Token 即将过期

由于 Token 具有一定的时效，在通话过程中如果 Token 即将失效，SDK 会提前 30 秒触发回调 `onTokenPrivilegeWillExpire` ，提醒应用程序更新 Token。当收到该回调时，用户需要重新在服务端生成新的 Token，然后调用 `renewToken` 将新生成的 Token 传给 SDK。

#### 3. 按用户返回音视频上下行码率、帧率、丢包率及延迟

为方便统计每个用户的音视频上下行码率、帧率及丢包率，该版本新增 `onRemoteAudioTransportStats` 和 `onRemoteVideoTransportStats` 回调。 通话或直播过程中，当用户收到远端用户发送的音视频数据包后，会周期性地发生该回调上报，频率约为 2 秒 1 次。 回调中包含用户的 UID、音/视频接收码率、丢包率、以及延迟时间（毫秒）。 并在统计频道内通话相关数据的 Rtcstats 类中增加 `lastmileDelay` 参数，返回客户端到 vos 服务器的延迟。

#### 4. 设置视频编码属性

为满足场景中视频旋转的需要，提升自定义视频源画质，该版本引入 `setVideoEncoderConfiguration` 替换原 `setVideoProfile` 接口，来设置视频编码属性。 新接口中的 `VideoEncoderConfiguration` 类对应一套视频参数，支持用户根据实际需要，手动设置视频的分辨率（dimension)、帧率 (frame rate)、码率 (bitrate) 以及视频方向 (orientationMode)。原接口 `setVideoProfile` 仍可使用，但不再推荐。

#### 5. 直播转码新增支持设置背景图片

在设置直播转码接口 `setLiveTranscoding` 中，新增 `backgroundImage` 参数，支持设置直播转码合图的背景图片。

### **改进功能**

-   优化了一对一音视频的质量，在降低延时、防止卡顿方面提升明显。优化效果重点覆盖东南亚、南美、非洲和中东等地区

-   直播场景下，改善了音频编码器的效率，保证通话质量的同时节省用户流量

-   采用深度学习算法，改进了通话及直播中的音频质量


### **问题修复**

-   修复了因视频编码问题引起的 Native 设备与 Web 端互通时，Web 端看不到 Native 端视频画面的问题

-   修复了多人视频直播连麦场景下，SDK 内存异常增长的问题

-   修复了某些 Android 设备上偶现的崩溃的问题

-   修复了特定场景下某些设备上偶发的无法看到远端视图的问题

-   修复了特定场景下部分 Android 设备上无法开启本地视频的问题

-   修复了特定场景下偶现的视频重影的问题

-   修复了小米 8 上出现的本地预览黑屏、远端也看不到它的问题

-   修复了通信模式下，由小流切换到大流时，偶现的视频画面下方出现绿边的问题。

-   修复了多平台互通时，一段时间后某些 Andorid 设备上偶现的崩溃问题。

-   修复了多人连麦场景下，主播频繁进出频道时，偶现的主播内存异常增长的问题

-   修复了某些 Android 设备上偶现的黑屏的问题

-   修复了特定场景下偶现的主播加入频道、下麦再上麦后，远端用户听不到主播声音的问题

-   修复了偶现的设置推流背景图无效的问题

-   修复了通信模式下，某些设备上偶现的视频画面长宽和设置的长宽颠倒的问题

-   修复了某些设备上偶现的开启视频模式加入频道后，调用 `destroy` 方法无响应的问题

-   修复了频道内其他用户频繁进出频道时，Android 设备上偶现的崩溃问题

-   修复了特定场景下某些 Android 设备上无法渲染远端视频，一直黑屏的问题

-   修复了特定场景下偶现的直播观众无法调节频道内通话音量的问题

-   修复了特定场景下某些 Android 设备上偶现的应用无响应的问题

-   修复了直播模式下，动态切换分辨率时，某些 Android 设备上偶现的崩溃的问题

-   修复了直播模式下，某些 Android 设备上出现的以观众身份加入频道，上麦后无法看到主播图像的问题

-   修复了通信模式下，反复设置不同的视频编码属性进出频道后，某些 Android 设备上出现码率爬升不上的问题

-   修复了直播场景下，主播和观众频繁切换角色时，观众切主播后偶现的采集不到画面的问题

-   修复了通信模式下，反复设置不同的视频编码属性进出频道后，某些 Android 设备上偶现的采集不到画面也不发流的问题

-   修复了设置手动对焦位置并触发对焦时，某些设备上偶现的崩溃的问题

-   修复了通信过程中，特定场景下某些 Android 设备上偶现的无法打开应用摄像头和系统摄像头的问题

-   修复了通信和直播模式下，某些 Android 设备上出现的加入频道一段时候后，视频卡住不再发流的问题

-   修复了 2 人连麦过程中，一端播放背景音乐时将自己静音或关闭音频后，另一端闪退的问题

-   修复了通信模式下，反复设置不同的视频编码属性后，无法进入频道的问题

-   修复了特定场景下，预加载音效时某些设备上偶现的崩溃问题

-   修复了特定场景下，某些 Android 设备上无法输出小分辨率视频的问题

-   修复了通信模式下，某些 Android 设备上偶现的服务端无法踢人的问题

-   修复了直播模式下，主播端编码与解码端渲染的分辨率不一致的问题

-   修复了某些 Andoid 设备上偶现的无法打开硬件编码器的问题

-   修复了通信和直播场景下偶发的视频画面卡住的问题

-   修复了进入频道前暂停指定用户视频流后，某些设备上偶现的崩溃的问题

-   修复了特定场景下，视频通话切到视频直播时，某些 Android 设备上出现的卡住的问题

-   修复了直播场景下，开关闪光灯时某些 Android 设备上出现崩溃的问题

-   修复了直播场景下，某些 Android 设备上出现的观众上麦后，主播接收不到上麦观众的音视频流的问题

-   修复了直播模式下，对某些外部视频源设置编码属性时，某些 Android 设备上出现的崩溃的问题

-   修复了直播模式下，对某些外部视频源设置编码属性时，某些 Android 设备上出现的输出视频方向不正确的问题

-   修复了特定场景下，某些 Android 设备上偶发的设置弱网下视频自动回退为 Audio-only 不生效的问题

-   修复了偶现的频繁切换 Token 时，某些 Android 设备上偶现的崩溃的问题

-   修复了特定场景下，某些 Android 设备上出现的分屏异常的问题

-   修复了直播场景下，旁路推流时部分 Android 设备上偶现的旁路观众看不到拉流画面的问题

-   修复了直播场景下，由连麦切换回单主播时偶现的画面卡顿的问题

-   修复了特殊场景下偶现的 SIP 设备和 SDK 视频不互通的问题

-   修复了某些 Android 设备上偶现的无法看到对方的问题

-   修复了部分 Android 设备上偶现的视频延迟的问题

-   修复了特定场景下某些 Android 设备上出现的传视频过程中出现的崩溃的问题


### **API 整理**

为提升用户体验，Agora 在 v2.3.0 版本中对 API 进行了梳理，并针对部分接口进行了如下处理：

为避免在直播转码推流中添加多个相同 User，以下接口在 v2.3.0 版本中进行删除，并将 `addUser` 返回类型由 void 变更为 int。

-   setUser


以下接口与录制相关，在 v2.3.0 版本后不再支持。Agora 提供专门的 Recording SDK 用于更好的录制服务，详见 [Agora Recording SDK 发版说明](../../cn/Product%20Overview/release_recording.md)。

-   `startRecordingService`

-   `stopRecordingService`

-   `refreshRecordingServiceStatus`

-   `onRefreshRecordingServiceStatus`


以下接口长期处于弃用状态，现进行删除，v2.3.0 版本后不再支持：

-   `monitorConnectionEvent`

-   `monitorBluetoothHeadsetEvent`

-   `monitorHeadsetEvent`

-   `setPreferHeadset`

-   `switchView`

-   `setSpeakerphoneVolume`


## **2.2.3 版**

该版本于 2018 年 7 月 5 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

为更好地提升用户体验，Agora SDK 在 v2.1.0 版本中对动态秘钥进行了升级。如果你当前使用的 SDK 是 v2.1.0 之前的版本，并希望升级到 v2.1.0 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。

### **问题修复**

-   修复了特定场景下偶发的线上统计崩溃的问题。

-   修复了直播时部分设备上主播声音变音的问题。

-   修复了直播时特定场景下偶发的崩溃的问题。

-   修复了多人直播连麦时，SDK 内存增长的问题。

-   修复了部分设备上偶发的离开频道后，收到 `onLeaveChannel` 回调偏慢的问题。

-   修复了偶发的无法正常反馈频道内谁在说话以及说话者音量的问题。

-   修复了直播时偶发的观众听到主播声忽大忽小的问题。

-   修复了直播时部分设备上偶发的视频卡住的问题。

-   修复了部分设备上偶发的关闭摄像头结束通话时，程序界面无响应的问题。

-   修复了特定场景下偶发的视频窗口尺寸变化后，视频卡住的问题。

## **2.2.2 版**

该版本于 2018 年 6 月 21 日发布。修复问题列表详见下文。

### **问题修复**

- 修复了特定场景下偶发的线上统计崩溃的问题
- 修复了部分安卓设备上偶发的音频崩溃的问题
- 修复了直播模式下部分安卓设备上主播声音变音的问题
- 修复了偶发的无法正常反馈频道内谁在说话以及说话者的音量的问题
- 修复了部分安卓设备上偶现的离开频道后，收到 `onLeaveChannel` 回调偏慢的问题
- 修复了特定场景下偶发的视频窗口尺寸变化后，视频卡住的问题
- 修复了部分安卓设备上偶现的关闭摄像头结束通话时，程序界面无响应的问题

## **2.2.1 版**

该版本于 2018 年 5 月 30 日发布。新增特性与修复问题列表详见下文。

### **问题修复**

-   修复了部分设备上偶现的游戏过程中 Crash 的问题。

-   修复了部分设备上无法获取声道指针的问题。

-   修复了部分设备上偶现的 Crash 问题。

-   修复了部分设备上插入耳机后无法调节音量的问题。


## **2.2.0 版**

该版本于 2018 年 5 月 4 日发布。新增特性与修复问题列表详见下文。

### **新增功能**

本次发版新增如下功能：

#### 1. 音效混响进频道

播放音效 `playEffect `接口新增了一个 `publish` 参数，用于在播放音效时，远端用户可以听到本地播放的音效。

> 如果你的 SDK 是由之前版本升级到 v2.2 版本，请务必关注该接口功能的变动。

#### 2. 服务端部署代理服务器

通过部署 Agora 提供的代理服务器安装包，设有企业防火墙的用户可以设置代理服务器，使用 Agora 的服务。详见 [企业部署代理服务器](../../cn/Quickstart%20Guide/proxy.md) 中的描述。

#### 3. 获取远端视频状态

新增 `onRemoteVideoStateChanged` 接口，以获知远端视频流的状态。

#### 4. 直播添加视频水印

在本地直播及旁路直播中增加水印功能，允许用户将一张 PNG 图片作为水印添加到正在进行的本地直播或旁路直播中。新增 `addVideoWatermark` 和 `clearVideoWatermarks` 接口，以添加或删除本地直播水印；`LiveTranscoding` 接口中新增 `watermark` 参数，用于控制旁路直播中水印的添加。

### **改进功能**

本次发版改进如下功能：

#### 1. 当前说话者音量提示

改进 `enableAudioVolumeIndication `接口的功能，无论频道内是否有人说话，都会在回调中按设置的时间间隔返回说话者音量提示。

#### 2. 频道内网络质量监测

根据用户对频道内实时网络质量监测的需求，在 `onNetworkQuality` 中改进了返回数据的准确度。

#### 3. 进入频道前网络条件监测

为方便用户在进频道前检查当前网络是否能支撑语音或视频通话，在 `onLastmileQuality` 中，由通过恒定码率监测优化为根据用户设定的 Video Profile 的码率进行监测，提高返回数据的准确度。且在网络状态为 unknown 时，依然以 2 秒的间隔返回回调。

#### 4. 提升音乐场景下的音质

提升了用户在播放音乐等场景下的音乐音质。

### **问题修复**

-   修复了大量用户同时直播连麦时，偶发的抖屏现象

-   修复了某些 App 在直播过程中退到后台后，旁路推流显示异常的问题


## **2.1.3 版**

该版本于 2018 年 4 月 19 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

该版本的 SDK 修改了`setVideoProfile` 方法在直播模式下的码率值，修改后的码率值与 2.0 版本一致。

### **问题修复**

修复了部分手机上，用户离开频道后，开启自带的录音设备时，偶现录音出错的问题。

### **改进**

改进了通信和直播模式下屏幕共享的效果，缩短了用户从屏幕共享模式切换回普通模式需要的时间间隔。

## **2.1.2 版**

该版本于 2018 年 4 月 2 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

SDK 升级至 2.1.2 的直播模式后，相同分辨率下，视频更清晰，但带宽也会变大。

### **新增功能**

在已有 `setVideoProfile `接口的基础上，新增一个同名接口。用户可以用此接口，根据自身业务需要，手动设置视频的分辨率、帧率和码率。

### **问题修复**

修复了之前版本 SDK 在 dtx + aac 模式下会视频卡顿的问题。

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

在直播场景下，主播如果需要通过内置的麦克风美化和定制自己的语音输入，可以通过调用 API `setLocalVoiceEqualization` 和 `setLocalVoiceReverb` 轻易地设置音效均衡和混响来实现所需要的效果。

#### 3. 在线频道信息查询

新增 Restful API 查询用户在频道中的状态信息，查询指定频道内的分角色用户列表，查询厂商频道列表，查询用户是否为连麦用户等。详见:

-   通话场景, 详见 [Dashboard RESTful API](../../cn/API%20Reference/dashboard_restful_communication.md)

-   互动直播场景, 详见 [Dashboard RESTful API](../../cn/API%20Reference/dashboard_restful_live.md)


#### 4. 17 人视频

在直播场景下，同一频道内支持 17 位主播同时进行视频直播和连麦，详见文档:

-   [实现视频直播](../../cn/Quickstart%20Guide/broadcast_video_android.md)

-   [实现七人以上视频通话](../../cn/Interactive%20Broadcast/seventeen_people_android.md)


#### 5. 自定义视频源

Agora SDK 提供了摄像头采集的默认实现，同时允许开发者使用自定义视频源。主要适用场景和文档详见 [自定义视频源和渲染器](../../cn/Quickstart%20Guide/custom_video_android.md) 。

#### 6. 自定义渲染器

Agora SDK 提供了默认的渲染器实现，用来显示本地视频图像和对端视频图像。使用默认的渲染器就能满足大部分开发者需求，复杂的业务场景下，Agora 也开放了自定义渲染器接口。

#### 7. 插入外部视频源

直播场景下，可以将采集到的视频添加到正在进行的直播中，直播室里的主播和观众可以一起边看电影、比赛或演出，边进行点评、互动等功能，会让现有的直播话题更广、体验更好。 仅支持拉入一路流，格式包括: RTMP, HLS, FLV。赛事直播最多同时支持 5 人连麦直播。详见 [外部输入直播视频源](../../cn/Quickstart%20Guide/inject_stream_android.md) 。

#### 8. 直播优化方案

提供一套全新的 API，直播场景优化 API，将原来 API 封装在底层，更快集成，更多功能扩展性。升级到 SDK 2.1 的用户可以选择使用新 API 或者老 API，两套方案均可以使用。

#### 9. 提示相机对焦区域

新增回调接口提示相机的对焦区域已发生改变，新增了回调 `onCameraFocusAreaChanged` 。

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
<tr><td>硬件编码优化</td>
<td>在 Qualcomm、MTK、Hisilicon、和 Orion 芯片上适配硬件 264 编码器</td>
</tr>
<tr><td>硬件编码优化</td>
<td>改善硬件编码器优化码率控制</td>
</tr>
<tr><td>计费优化</td>
<td>计费系统针对极小分辨率统一按音频计费，例如 16 * 16</td>
</tr>
</tbody>
</table>



### **问题修复**

-   修复了华为 Nexus 6p 播放杂音的问题。

-   修复了一加手机上的破音问题。

-   修复了自采集声音不正常问题。

-   修复了偶现的崩溃问题。


## **2.0.2 版**

该版本于 2017 年 12 月 15 日发布。新增特性与修复问题列表详见下文。

### **问题修复**

修复了偶现的语音路由问题。

## **2.0 版及之前**

### **2.0 版**

该版本于 2017 年 12 月 6 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   通信场景支持视频大小流功能，新增 API `setRemoteVideoStreamType` 和 `enableDualStreamMode`

-   通信和直播场景下支持摄像头管理功能，新增以下 API:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>isCameraZoomSupported</code></td>
<td>检测设备是否支持相机缩放功能</td>
</tr>
<tr><td><code>isCameraTorchSupported</code></td>
<td>检测设备是否支持闪光灯常开</td>
</tr>
<tr><td><code>isCameraFocusSupported</code></td>
<td>检测设备是否支持手动对焦功能</td>
</tr>
<tr><td><code>isCameraAutoFocusFaceModeSupported</code></td>
<td>检测设备是否支持人脸对焦功能</td>
</tr>
<tr><td><code>setCameraZoomFactor</code></td>
<td>设置相机缩放因子</td>
</tr>
<tr><td><code>getCameraMaxZoomFactor</code></td>
<td>获取相机支持最大缩放比例</td>
</tr>
<tr><td><code>setCameraFocusPositionInPreview</code></td>
<td>设置手动对焦位置，并触发对焦</td>
</tr>
<tr><td><code>setCameraTorchOn</code></td>
<td>设置是否打开闪光灯</td>
</tr>
<tr><td><code>setCameraAutoFocusFaceModeEnabled</code></td>
<td>设置是否开启人脸对焦功能</td>
</tr>
</tbody>
</table>



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



-   通信和直播场景下支持服务端踢人功能。

-   新增以下 Android 模拟器支持: 夜神、雷电、逍遥。


#### **改进**

硬件编码器优化: 支持超小分辨率的编码，例如 64 x 64。

#### **问题修复**

-   修复了音频路由和蓝牙相关的若干问题。

-   优化了音量均衡控制。


### **1.14 版**

该版本于 2017 年 10 月 20 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `setAudioProfile 设`置音频参数和应用场景。

-   新增 API `setLocalVoicePitch` 提供基础变声功能。

-   直播场景: 新增 API `setInEarMonitoringVolume`提供调节耳返音量功能。


#### **改进**

-   优化了在高码率下的音频体验。

-   秒开: 直播场景下，单流模式时观众加入频道 1 秒内看见主播图像 (均值为 226 ms, 网络状态良好时可达 204 ms)。

-   节省带宽:

    -   1.14 以前: 如果你选择不听某人的音频或不看某人的视频，音视频流会照发。

    -   1.14 开始: 如果你选择不听或不看某人的流，则不会下发，从而节省带宽。

-   精准的码率控制:

    -   1.14 以前: 码率控制不够精准，上下波动幅度较大。波动过大容易造成网络拥塞，增加丢包、丢帧的概率，影响了带宽估计模块的精度，特别是在弱网低码率情况下尤为明显。

    -   1.14 开始: 精准的码率控制，要多少给多少，不多给也不少给，避免波动过大造成的网络拥塞，减少传输延时，有助于减少网络卡顿。


#### **问题修复**

修复了部分 Android 机器上摄像头相关的问题。

### **1.13.1 版**

该版本于 2017 年 9 月 28 日发布。新增特性与修复问题列表详见下文。

#### **优化**

优化了特定场景下出现的回声问题。

### **1.13 版**

该版本于 2017 年 9 月 4 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `onClientRoleChanged` 用于提醒直播场景下主播、观众上下麦的回调。

-   新增功能 Android 模拟器。

-   新增单独关闭语音播放的功能。

-   新增功能支持服务端推流失败回调。


#### **改进**

-   软编情况下，视频属性可控。

-   可以在客户端设置推流的 Profile。


#### **修复问题**

修复了部分机型上偶现的崩溃

### **1.12 版**

该版本于 2017 年 7 月 25 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   在 API 方法 `setEncryptionMode`里新增了加密模式 `aes-128-ecb`。

-   在 API 方法 `startAudioRecording` 里新增了参数 `quality` 用于设置录音音质。

-   新增了一系列 API 管理音效。

-   直播场景下， 新增了 API 方法 `injectStream`在当前频道内插入一条 RTMP 流。该功能目前为 beta 版。


#### **改进**

通信场景下针对 320 x 180 分辨率提供了以下改进方案:

-   网络和设备状态较差的情况下仍能保证画质流畅度。

-   网络和设备状态良好的情况下可以做到比 180P 更好的画质清晰度。


#### **修复问题**

-   修复了部分机型上蓝牙相关的语音路由问题。

-   修复了部分机型上偶现的崩溃问题。



