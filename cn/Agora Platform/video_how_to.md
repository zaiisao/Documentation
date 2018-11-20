
---
title: 视频相关
description: 
platform: 视频相关
updatedAt: Tue Nov 20 2018 09:33:19 GMT+0000 (UTC)
---
# 视频相关
### 我该如何选择视频分辨率、帧率、码率？

通常来讲，视频参数的选择要根据产品实际情况来确定，比如，如果是1对1 ，老师和学生的窗口比较大，要求分辨率会高一点，随之帧率和码率也要高一点；如果是1对4， 老师和学生的窗口都比较小，分辨率可以低一点，对应的码率帧率也会低一点，以减少编解码的资源消耗和缓解下行带宽压力。一般可按下列场景中的推荐值进行设置。

* 2 人视频通话场景：
   *  分辨率 320 * 240、帧率 15 fps、码率 200 Kbps
   *    分辨率 640 * 360、帧率 16fps、码率 400 Kbps
* 多人视频通话场景：
   *    分辨率 160 * 120、帧率 15 fps、码率 65 Kbps
   *    分辨率 320 * 180、帧率 15 fps、码率 140 Kbps
   *    分辨率 320 * 240、帧率 15 fps、码率 200 Kbps

如果你希望自定义视频参数，比如调高码率以保证视频质量，也可以使用 `setVideoEncoderConfiguration` 对各参数进行自定义设置。
通常的，直播场景下需要较大码率来提升视频质量。因此 Agora 建议将直播码率值设为通信值的 2 倍。详情请[参考设置码率](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1video_1_1_video_encoder_configuration.html#a4b090cd0e9f6d98bcf89cb1c4c2066e8) 。

### 我该如何调整参数获取我想要的视频方向？

为满足用户处理视频旋转的需求，Agora 为用户提供了设置视频编码属性 `setVideoEncoderConfiguration` 接口，其中就包含了 `orientationMode` 参数，即旋转模式。

Agora 的视频采集、渲染和输出的流程大致如下：

<img alt="../_images/rotation_encoding_decoding.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_encoding_decoding.jpg" />

因此在视频旋转场景中，我们主要关注两个端：采集端和播放端 。

* 采集端：采集端采集视频，并输出视频信息。视频信息包含视频图像和视频与 Status Bar 的相对位置
* 播放端：播放端负责播放接收到的视频图像

Agora 通过 `orientaionMode` 参数，提供了 ADAPTIVE 模式、FIXED_LANDSCAPE 和 FIXED_PORTRAIT 三种模式，供用户在不同场景下使用。**无论是哪一种模式，Agora SDK 保证视频和 Status Bar 的相对位置在采集端和播放端始终一致**。

Agora 推荐根据下表来选择适合你场景的旋转模式（通信和直播模式均适用）：

<img alt="../_images/rotation_mode.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_mode.jpg" />

#### ADAPTIVE 模式

该模式下，采集端采集视频和视频与 Status Bar 的相对方向信息并传输；播放端播放视频并还原视频与 Status Bar 的相对方向信息。整个过程中不裁剪硬件采集的视频。

假设视频采集设备使用后置摄像头进行采集，下图演示了 Adaptive 模式分别在 UI 锁定和 UI 不锁定情况下的行为：

-   UI 锁定时（或 UI 不锁定但客户端关闭了屏幕自动旋转功能时）：

    Status Bar 与屏幕的相对方向保持不变，和手机的重力感应无关（比如微信，Status Bar 总是位于屏幕的顶端）。此时，视频和屏幕的相对方向在采集端和播放端始终一致：

    采集端横屏时：

    <img alt="../_images/rotation_adaptive_uilock_landscape.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_adaptive_uilock_landscape.jpg" />

    采集端竖屏时：

    <img alt="../_images/rotation_adaptive_uilock_portrait.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_adaptive_uilock_portrait.jpg" />


-   UI 不锁定且客户端开启屏幕自动旋转时：

    Status Bar 总是处于垂直地面方向的正上方，和屏幕的朝向无关（比如 Facetime）。此时，视频和重力的相对方向在采集端和播放端始终一致：

    采集端横屏时：

    <img alt="../_images/rotation_adaptive_uiunlock_landscape.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_adaptive_uiunlock_landscape.jpg" />

     采集端竖屏时：

    <img alt="../_images/rotation_adaptive_uiunlock_portrait.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_adaptive_uiunlock_portrait.jpg" />

#### FIXED_LANDSCAPE 模式

该模式下，采集端保证输出的视频相对 Status Bar 处于横屏模式；如有需要，会对硬件采集的视频进行裁剪处理。播放端不对视频进行旋转操作，直接渲染出横屏画面。

> FIXED\_LANDSCAPE 模式下，硬件采集到的视频在垂直于 Status Bar 的两端可能会被裁剪。

假设视频采集设备使用后置摄像头进行采集，下图演示了 FIXED\_LANDSCAPE 模式在采集端处于不同角度时的行为：

-   采集到的视频是横屏模式：（采集端未对硬件采集的视频进行裁剪处理）

    <img alt="../_images/rotation_fixed_landscape.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_fixed_landscape.jpg" />


-   采集到的视频是竖屏模式：（采集端对硬件采集的视频进行裁剪处理，使其成为横屏画面。图中红色虚线部分演示 SDK 对采集到的图像裁剪后留下的部分）

    <img alt="../_images/rotation_fixed_landscape_cut.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_fixed_landscape_cut.jpg" />

#### FIXED_PORTRAIT 模式

该模式下，采集端保证输出的视频相对 Status Bar 处于竖屏模式；如果需要，会对硬件采集的视频进行裁剪处理。播放端不对视频进行旋转操作，直接渲染出竖屏画面。

> FIXED\_PORTRAIT 模式下，硬件采集到的视频在平行于 Status Bar 的两端可能会被裁剪。

假设视频采集设备使用后置摄像头进行采集，下图演示了 FIXED\_PORTRAIT 模式在采集端处于不同角度时的行为：

-   采集到的视频是竖屏模式：（采集端未对硬件采集的视频进行裁剪处理）

    <img alt="../_images/rotation_fixed_portrait.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_fixed_portrait.jpg" />


-   采集到的视频是横屏模式：（采集端对硬件采集的视频进行裁剪处理，使其成为竖屏画面。图中红色虚线部分演示 SDK 对采集到的图像裁剪后留下的部分）

    <img alt="../_images/rotation_fixed_portrait_cut.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_fixed_portrait_cut.jpg" >


