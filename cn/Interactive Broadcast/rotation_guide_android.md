
---
title: 视频采集旋转
description: 
platform: Android,Windows
updatedAt: Tue Dec 11 2018 09:56:32 GMT+0800 (CST)
---
# 视频采集旋转
本文指导用户如何选择与场景适配的视频旋转模式。

为满足用户处理视频旋转的需求，Agora 为用户提供了设置视频编码属性 `setVideoEncoderConfiguration` 接口，其中就包含了 `orientationMode` 参数，即旋转模式。

## 视频旋转实现

Agora 的视频采集、渲染和输出的流程大致如下：

<img alt="../_images/rotation_encoding_decoding.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_encoding_decoding.jpg" style="width: 840px; "/>


因此在视频旋转场景中，我们主要关注两个端：采集端和播放端 。

### 采集端

采集端采集视频，并输出视频信息。视频信息包含：

-   视频图像

-   视频与 Status Bar 的相对位置


### 播放端

播放端负责播放接收到的视频图像。

## 接口实现及参数选择

进入 Agora 频道后，用户可以通过调用 `setVideoEncoderConfiguration` 来设置视频属性。

```
public abstract int setVideoEncoderConfiguration(VideoEncoderConfiguration config);
```

其中的 **VideoEncoderConfiguration** 就包含 `orientationMode` 参数。Agora 推荐根据下表来选择适合你场景的旋转模式（通信和直播模式均适用）：

<img alt="../_images/rotation_mode.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_mode.jpg" style="width: 840px; "/>


## 视频旋转模式

Agora 通过 `orientaionMode` 参数，提供了 [ADAPTIVE 模式](#orientation_adaptive) 、 [FIXED\_LANDSCAPE 模式](#orientation_fixed_landscape) 和 [FIXED\_PORTRAIT 模式](#orientation_fixed_portrait) 三种模式，供用户在不同场景下使用。**无论是哪一种模式，Agora SDK 保证视频和 Status Bar 的相对位置在采集端和播放端始终一致**。

<a name = "orientation_adaptive"></a>
### ADAPTIVE 模式

该模式下，采集端采集视频和视频与 Status Bar 的相对方向信息并传输；播放端播放视频并还原视频与 Status Bar 的相对方向信息。

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


<a name = "orientation_fixed_landscape"></a>
### FIXED\_LANDSCAPE 模式

该模式下，采集端保证输出的视频相对 Status Bar 处于横屏模式；如有需要，会对硬件采集的视频进行裁剪处理。播放端不对视频进行旋转操作，直接渲染出横屏画面。

> FIXED\_LANDSCAPE 模式下，硬件采集到的视频在垂直于 Status Bar 的两端可能会被裁剪。

假设视频采集设备使用后置摄像头进行采集，下图演示了 FIXED\_LANDSCAPE 模式在采集端处于不同角度时的行为：

-   采集到的视频是横屏模式：（采集端未对硬件采集的视频进行裁剪处理）

    <img alt="../_images/rotation_fixed_landscape.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_fixed_landscape.jpg" />


-   采集到的视频是竖屏模式：（采集端对硬件采集的视频进行裁剪处理，使其成为横屏画面。图中红色虚线部分演示 SDK 对采集到的图像裁剪后留下的部分）

    <img alt="../_images/rotation_fixed_landscape_cut.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_fixed_landscape_cut.jpg" />


<a name = "orientation_fixed_portrait"></a>
### FIXED\_PORTRAIT 模式

该模式下，采集端保证输出的视频相对 Status Bar 处于竖屏模式；如果需要，会对硬件采集的视频进行裁剪处理。播放端不对视频进行旋转操作，直接渲染出竖屏画面。

> FIXED\_PORTRAIT 模式下，硬件采集到的视频在平行于 Status Bar 的两端可能会被裁剪。

假设视频采集设备使用后置摄像头进行采集，下图演示了 FIXED\_PORTRAIT 模式在采集端处于不同角度时的行为：

-   采集到的视频是竖屏模式：（采集端未对硬件采集的视频进行裁剪处理）

    <img alt="../_images/rotation_fixed_portrait.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_fixed_portrait.jpg" />


-   采集到的视频是横屏模式：（采集端对硬件采集的视频进行裁剪处理，使其成为竖屏画面。图中红色虚线部分演示 SDK 对采集到的图像裁剪后留下的部分）

    <img alt="../_images/rotation_fixed_portrait_cut.jpg" src="https://web-cdn.agora.io/docs-files/cn/rotation_fixed_portrait_cut.jpg" />
