
---
title: 发版说明
description: 
platform: Windows
updatedAt: Fri Nov 30 2018 04:03:05 GMT+0000 (UTC)
---
# 发版说明

本文提供 Agora 视频 SDK 的发版说明。

## **简介**

Windows 视频 SDK 支持两种主要场景:

-   音视频通话

-   音视频直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video?platform=All%20Platforms) 以及[互动直播产品概述](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)了解关键特性。


将下载的 Windows 软件包解压后, 你会看见两个包，根据实际情况选择使用 x64 或 x86 包:

<img alt="../_images/windows_package.png" src="https://web-cdn.agora.io/docs-files/cn/windows_package.png" style="width: 300px; "/>


解压 x64 或 x86 后，包内的 examples 文件夹下有两个示例代码供你演示功能使用: AgoraOpenLive 和 OpenVideoCall 。 在编译示例代码时须确保编译环境选对。例如，x64 编译环境选择 x64:

<img alt="../_images/x64.png" src="https://web-cdn.agora.io/docs-files/cn/x64.png" style="width: 300px; "/>


x86 的编译环境选择 Win32:

<img alt="../_images/x86.png" src="https://web-cdn.agora.io/docs-files/cn/x86.png" style="width: 300px; "/>


## **2.2.0 版**

该版本于 2018 年 5 月 4 日发布。新增特性与修复问题列表详见下文。

**升级必看**

为更好地提升用户体验，Agora SDK 在 2.1 版本中对动态秘钥进行了升级。 如果你当前使用的 SDK 是 2.1 之前的版本，并希望升级到 2.1 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。

**新增功能**

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

**改进功能**

本次发版改进如下功能：

#### 1. 当前说话者音量提示

改进 `enableAudioVolumeIndication` 接口的功能，无论频道内是否有人说话，都会在回调中按设置的时间间隔返回说话者音量提示。

#### 2. 频道内网络质量监测

根据用户对频道内实时网络质量监测的需求，在 `onNetworkQuality` 中改进了返回数据的准确度。

#### 3. 进入频道前网络条件监测

为方便用户在进频道前检查当前网络是否能支撑语音或视频通话，在 `onLastmileQuality` 中，由通过恒定码率监测优化为根据用户设定的 Video Profile 的码率进行监测，提高返回数据的准确度。且在网络状态为 unknown 时，依然以 2 秒的间隔返回回调。

#### 4. 提升音乐场景下的音质

提升了用户在播放音乐等场景下的音乐音质。

**修复问题**

-   修复了大量用户同时直播连麦时，偶发的抖屏现象

-   修复了 Windows 设备在直播模式下偶发的音频卡顿问题


## **2.1.3 版**

该版本于 2018 年 4 月 19 日发布。新增特性与修复问题列表详见下文。

**升级必看**

该版本的 SDK 修改了 `setVideoProfile` 方法在直播模式下的码率值，修改后的码率值与 2.0 版本一致。

**问题修复**

修复了部分手机上，用户离开频道后，开启自带的录音设备时，偶现录音出错的问题。

**改进**

改进了通信和直播模式下屏幕共享的效果，缩短了用户从屏幕共享模式切换回普通模式需要的时间间隔。

## **2.1.2 版**

该版本于 2018 年 4 月 2 日发布。新增特性与修复问题列表详见下文。

**升级必看**

SDK 升级至 2.1.2 的直播模式后，相同分辨率下，视频更清晰，但带宽也会变大。

**问题修复**

-   修复了连麦后退出频道后再进入频道连麦对端看不到自己的问题。

-   修复了通信模式的屏幕共享清晰度小于直播模式的问题。


## **2.1.1 版**

该版本于 2018 年 3 月 16 日发布。

请正在或已集成 2.1 SDK 的客户尽快升级更新！ 本次发版修复了一个的系统风险，请尽快升级以免对服务造成影响。

## **2.1.0 版**

该版本于 2018 年 3 月 7 日发布。新增特性与修复问题列表详见下文。

**新增功能**

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

-   [实现 17 人直播场景](../../cn/Quickstart%20Guide/seventeen_people.md)


#### 5. 插入外部视频源

直播场景下，可以将采集到的视频添加到正在进行的直播中，直播室里的主播和观众可以一起边看电影、比赛或演出，边进行点评、互动等功能，会让现有的直播话题更广、体验更好。 仅支持拉入一路流，格式包括: RTMP, HLS, FLV。赛事直播最多同时支持 5 人连麦直播。详见 [外部输入直播视频源](../../cn/Quickstart%20Guide/inject_stream_windows.md) 。


#### 6. 新增直播场景下的屏幕共享功能

-   在 2.1.0 以前: Agora SDK 仅支持视频通话场景下的屏幕共享功能;

-   从 2.1.0 开始: Agora SDK 正式支持直播场景下的屏幕共享功能;


#### 7. 声卡采集

新增 API `enableLoopbackRecording` 开启视声卡采集，开启后 SDK 可以采集到本地播放的所有声音。

**改进**

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



**问题修复**

-   修复了摄像头采集失败问题;

-   修复了偶现的崩溃;

-   修复了偶现的视频卡顿问题;


**2.0 预告**

致各位开发者:

由于 Agora 的产品升级， 当 Windows SDK 升级至 2.0 将不支持 Agora Recording Server（<=1.8.2）的调用接口，相关 API 将被废弃。

#### 受影响范围

-   使用 Windows SDK 并且使用 Agora Recording Server（<= version 1.8.2）的用户

-   涉及到的 API 为 `startRecordingService`，`stopRecordingService` 以及 `refreshRecordingServiceStatus`，将在后续版本中不再支持。


#### 解决方案

我们提供了两种方案供您选择:

**方案一**: 将 Agora Recording Server（<=version 1.8.2）升级到 Agora Recording SDK\( \>= version 1.12\)。Agora Recording SDK 无需在客户端触发录制，将不影响 Windows SDK 的后续升级（推荐该方法）

附上 Agora Recording SDK 的使用方法:

-   [录制快速开始](../../cn/Quickstart%20Guide/recording_cpp-1.md)

-   [录制音视频](../../cn/Quickstart%20Guide/recording_voice_video.md)

-   [录制 API](../../cn/API%20Reference/recording_cpp.md)


**方案二**：如果你希望继续使用 Agora Recording Server，维持 Windows SDK 版本不变（<=v1.14），将不影响您继续使用 Windows SDK的 API 触发录制。

若您有任何疑问，可以通过以下方式获得技术支持:

1.  通过 Agora 开发者社区提问: [https://dev.agora.io/cn/](https://dev.agora.io/cn/)

2.  通过 Agora 工单系统提交工单

3.  您还可以随时联系技术支持群里的同事或商务


## **2.0 版**

该版本于 2017 年 12 月 6 日发布。新增特性与修复问题列表详见下文。

**新增功能**

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




**问题修复**

修复了加入频道后出现的崩溃。

## **1.14 版**

该版本于 2017 年 10 月 20 日发布。新增特性与修复问题列表详见下文。

**新增功能**

-   新增 API `setAudioProfile `设置音频参数和应用场景。

-   新增 API `setLocalVoicePitch` 提供基础变声功能。

-   直播场景: 新增 API `setInEarMonitoringVolume` 提供调节耳返音量功能。


**改进**

-   优化了在高码率下的音频体验。

-   秒开: 直播场景下，单流模式时观众加入频道 1 秒内看见主播图像\(均值为 885 ms, 网络状态良好时可达 788 ms\)。

-   节省带宽:

    -   1.14 以前: 如果你选择不听某人的音频或不看某人的视频，音视频流会照发。

    -   1.14 开始: 如果你选择不听或不看某人的流，则不会下发，从而节省带宽。

-   精准的码率控制:

    -   1.14 以前: 码率控制不够精准，上下波动幅度较大。波动过大容易造成网络拥塞，增加丢包、丢帧的概率，影响了带宽估计模块的精度，特别是在弱网低码率情况下尤为明显。

    -   1.14 开始: 精准的码率控制，要多少给多少，不多给也不少给，避免波动过大造成的网络拥塞，减少传输延时，有助于减少网络卡顿。


**问题修复**

-   修复了部分 Windows 机器上无声音的问题。

-   修复了部分 Windows 机器上的摄像头问题。


## **1.13.1 版**

该版本于 2017 年 9 月 28 日发布。新增特性与修复问题列表详见下文。

**优化**

优化了特定场景下出现的回声问题。

## **1.13 版**

该版本于 2017 年 9 月 4 日发布。新增特性与修复问题列表详见下文。

**新增功能**

-   新增 API `onClientRoleChanged` 用于提醒直播场景下主播、观众上下麦的回调。

-   新增单独关闭语音播放的功能。

-   新增功能支持服务端推流失败回调。

-   屏幕共享直播场景下采集声卡选项动态开启和关闭。


**改进**

-   软编情况下，视频属性可控。

-   可以在客户端设置推流的 Profile。

-   屏幕共享: 提升了画质清晰度和流畅度。

-   屏幕共享: 通信场景下新增动态更新截图区域。


**修复问题**

修复了部分机型上偶现的崩溃。

## **1.12 版**

该版本于 2017 年 7 月 25 日发布。新增特性与修复问题列表详见下文。

**新增功能**

-   直播场景下， 新增 API 方法 `injectStream` 在当前频道内插入一条 RTMP 流。该功能目前为 beta 版

-   在 API 方法 `setEncryptionMode` 里新增加密模式 `aes-128-ecb` 。

-   在 API 方法 `startAudioRecording` 里新增参数 `quality`用于设置录音音质。

-   新增 API `onActiveSpeaker` 提示当前频道内谁在说话。

-   通信场景下，删除了原有的 API 方法 `setScreenCaptureWindow`，更新 API 方法 `startScreenCapture` 共享整个屏幕、指定窗口或指定区域

-   通信场景下，启用屏幕共享功能后，在屏幕共享过程中可以显示鼠标


**改进**

通信场景下针对 320 x 180 分辨率提供了以下改进方案:

-   网络和设备状态较差的情况下仍能保证画质流畅度。

-   网络和设备状态良好的情况下可以做到比 180P 更好的画质清晰度。


**修复问题**

修复了部分机型上偶现的崩溃问题。


