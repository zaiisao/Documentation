
---
title: 发版说明
description: 
platform: 微信小程序
updatedAt: Mon Sep 21 2020 10:18:02 GMT+0800 (CST)
---
# 发版说明
本文提供声网 Agora 小程序 SDK 的发版说明。

## **简介**

声网 Agora 小程序 SDK 支持微信小程序实现以下功能, 并能与声网其他 SDK 进行互通：

-   音视频通话
-   音视频直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video?platform=All%20Platforms) 以及[互动直播产品概述](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)了解关键特性。


结合微信小程序，能实现如下场景：

-   线上课堂：1 对 1 及 1 对多线上小班课，老师、学生实时互动
-   在线医疗：突破医疗资源的地域限制，实现多方视频会诊，降低诊断成本
-   高端客服：对高价值的 VIP 客户提供远程视频服务，1 对 1 实时交流
-   远程报警：一键报警，通过实时视频通信，为警方提供一手现场情况
-   银行开户：实时视频认证，清晰画质、超低延时、隐私保护，提升开户效率

## 技术方案

下图为小程序连麦的声网实现架构图：

<img alt="../_images/wechat_live_solution.jpg" src="https://web-cdn.agora.io/docs-files/cn/wechat_live_solution.jpg" style="width: 601.6px; height: 240.8px;"/>

在这个架构图中：

1.  在 Agora SD-RTN 边缘节点部署协议转换服务对小程序端发出的 RTMP 流进行转换；
2.  将转化后的 UDP 传输到 Agora SD-RTN 上；
3.  通过 Agora SD-RTN 与 Agora 其他平台 SDK 实现音视频互通。

点击 [声网小程序 Demo 体验](../../cn/Audio%20Broadcast/start_call_wechat.md) 了解小程序通话、互通等功能。
下载小程序可供集成的示例代码，请前往 [https://github.com/AgoraIO/Agora-Miniapp-Tutorial](https://github.com/AgoraIO/Agora-Miniapp-Tutorial) 。

## **2.4.5 版**

该版本于 2020 年 9 月 21 日发布。

**新增功能**

#### 远端用户停止发送视频回调

为方便本地了解远端用户的媒体状态，该版本新增 `Client.on("mute-video")` 回调。当远端用户停止发送视频流时，SDK 会触发相应的回调。你还可以从该回调中得知停止发送的远端用户的 ID。

## **2.4.4 版**

该版本于 2020 年 7 月 7 日发布。增加了对 QQ 小程序的兼容。

## **2.4.3 版**

该版本于 2020 年 5 月 26 日发布。

**新增功能**

#### 跨频道媒体流转发

跨频道媒体流转发，指将主播的媒体流转发至其他频道，实现主播与其他频道的主播实时互动的场景。该版本新增如下方法，通过将源频道中的媒体流转发至目标频道，实现跨直播间连麦功能：

- `startChannelMediaRelay`
- `updateChannelMediaRelay`
- `stopChannelMediaRelay`

在跨频道媒体流转发过程中，SDK 会通过 `Client.on("channel-media-relay-state")` 和 `Client.on("channel-media-relay-event")` 回调报告媒体流转发的状态和事件。

## **2.4.2 版**

该版本于 2019 年 12 月 31 日发布。

**新增功能**

#### 远端用户停止发送音回调

为方便本地了解远端用户的媒体状态，该版本新增 `Client.on("mute-audio")`。当远端用户停止发送音频流时，SDK 会触发相应的回调。你还可以从该回调中得知停止发送的远端用户的 ID。

## **2.4.1 版**

该版本于 2019 年 9 月 18 日发布。进行了一些内部优化。

## **2.4.0 版**

该版本于 2019 年 4 月 30 日发布。新增特性和性能改进详见下文。

**新增功能**

为方便用户控制本地是否发流，该版本新增 [`Client.muteLocal`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#mutelocal) 和 [`Client.unmuteLocal`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#unmuteLocal) 方法。用户可以使用该方法在小程序中实现不发送本地音视频流等功能。

**性能改进**

该版本针对直播下主播切换为观众的场景，对图像和声音的延迟进行了优化。声网实验室测试结果显示，延迟降低为之前的 20%。

## **2.3.2 版**

该版本于 2019 年 1 月 3 日发布。新增特性及功能改进详见下文。

**新增功能**

为方便用户了解是否成功调用 [`Client.leave`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#leave) 和 [`Client.destroy`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#destroy) 方法，该版本分别在这两个方法中新增了方法调用成功或失败的回调函数。

**改进**

为提升 [`Client.setRole`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#setRole) 方法的易用性，该版本优化了该方法的调用逻辑。用户在调用 [`Client.join`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#join) 加入频道**前**或**后**均可以调用该方法，设置或改变用户角色。

同时，该版本对 `broadcaster` 和 `audience` 两个角色的行为进行了更为严格的定义：

- 用户角色为 `broadcaster` 时，可以调用 [`Client.publish`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#publish) 和 [`Client.unpublish`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#unpublish) 方法
- 用户角色为 `audience` 时，不可以调用 [`Client.publish`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#publish) 和 [`Client.unpublish`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/interfaces/client.html#unpublish) 方法


## **1.1.3 Beta 版**

该版本于 2018 年 11 月 2 日发布。修复问题详见下文。

**问题修复**

修复了偶现的反复加入频道失败的问题，提升了服务的稳定性。

## **1.1.2 Beta 版**

该版本于 2018 年 10 月 22 日发布。新增特性与修复问题列表详见下文。

**新增功能**

为方便用户在通话或直播过程中，控制是否订阅远端音视频流，该版本新增如下接口：

- `Client.mute`：停止接收远端音视频流
- `Client.unmute`：继续接收远端音视频流

通过使用这两个接口，用户可以选择停止或继续接收音频流、视频流还是音视频流。详细用法见 [微信小程序 API 参考](https://docs.agora.io/cn/Video/API%20Reference/wechat/index.html)。

**改进**

通过添加如下安全域名，实现高可用策略，提升小程序服务的稳定性：

https://miniapp-1.agoraio.cn
https://miniapp-2.agoraio.cn
https://miniapp-3.agoraio.cn
https://miniapp-4.agoraio.cn


## **1.1.1 Beta 版**

该版本于 2018 年 8 月 8 日发布。新增特性与修复问题列表详见下文。

**新增功能**

为提升互通体验，该版本新增了 `setRole` 接口。当使用场景同时满足以下条件时，小程序 SDK 必须调用该接口将用户角色设置为观众：

-   小程序 SDK 与 Native SDK 互通
-   Native 端的频道场景为直播场景
-   小程序端作为观众加入频道


> 该方法必须在 `join`  加入频道前调用才能生效。

## **1.1.0 Beta 版**

该版本于 2018 年 7 月 27 日发布。新增特性与修复问题列表详见下文。


**包含功能**

-   支持音视频通话及互通直播场景
-   支持万人频道，小程序最多支持 7 个视频画面
-   支持小程序 SDK 与 Native SDK、Web SDK 之间的双向互通。**提供业界最优质的小程序和 Web 互通体验。**


**已知问题与局限**

由于小程序基于微信平台搭建，很多功能在实现中会有局限。目前已知的问题和局限如下所示：

-   推荐 7 人及 7 人以下的音视频通话或直播场景。频道内人数越少，效果越好。
-   推荐 8 人及 8 人以下的纯音频场景，但是功能上支持到 17 人，对网络要求非常高。频道内人数越少，效果越好。
-   通信场景下，小概率出现远端视频窗口画面旋转的问题。
-   纯音频场景下，如果用户一进频道就禁用麦克风，后加入的用户在你打开麦克风说话前可能无法感知频道内你的存在。


经测试，如下为微信小程序的固有问题，有可能会随微信小程序的版本升级而得到解决：

-   由于微信小程序使用 RTMP 协议，弱网情况下，可能会出现音视频快放、声音忽大忽小、偶有回声、推流中断、网络错误等现象。
-   部分 iOS 手机会偶现编码失败，导致远端用户看到黑屏现象。
-   部分 iOS 手机多人互通长时间会出现异常退出微信小程序以及微信崩溃问题。
-   通信或直播过程中，个别手机上会出现摄像头画面卡住的问题。目前监测到该问题的手机有小米 5S。
-   切到微信后台一段时间再返回小程序时，小概率出现问题。
-   正常的音视频或纯语音场景下，不影响正常通话，但测试结论不推荐播放背景音乐，可能会出现声音卡顿、忽大忽小等不可控现象。
-   iOS 手机系统为最新的 iOS 12.0 的用户使用微信小程序时，对方看到该用户的窗口必现花屏。



