
---
title: Agora 关键术语
description: 
platform: All Platforms
updatedAt: Tue Oct 29 2019 02:59:58 GMT+0800 (CST)
---
# Agora 关键术语
阅读本文了解 Agora 平台的关键术语。

## 基本概念

使用 Agora SDK 之前需要了解的基本概念。

### RTC SDK

我们将能实现音视频实时通信功能的 Agora SDK 统称为 RTC (Real-Time Communication) SDK，能实现[语音通话](https://docs.agora.io/cn/Voice/product_voice)、[视频通话](https://docs.agora.io/cn/Video/product_video)、[音频互动直播](https://docs.agora.io/cn/Audio%20Broadcast/product_live_audio)以及[视频互动直播](https://docs.agora.io/cn/Interactive%20Broadcast/product_live)等实时通信场景。 按照平台，我们把 RTC SDK 分为以下几类：

- Agora Native SDK，包括 Android，iOS，macOS，Windows 以及 Electron 平台
- Agora Web SDK
- Agora 微信小程序 SDK

### 控制台

[控制台](https://dashboard.agora.io/)是 Agora 提供给用户创建和管理项目的平台。[注册账号](https://dashboard.agora.io/cn/signup)之后，你可以通过[控制台](https://dashboard.agora.io/)创建项目、获得 [App ID](#appid)、查看通话用量、分析通话质量以及查看账单。

### <a name="appid"></a>App ID

Agora 给应用程序开发人员分配 App ID，以识别项目和组织。在 [控制台](https://dashboard.agora.io/) 注册后，你可以创建多个项目，每个项目都有一个唯一的 App ID。详见[获取 App ID](../../cn/Agora%20Platform/token.md)。

不同的 App ID 在 Agora 实时网络中的通话是完全独立的。因此，不同 App ID 的项目无法相互通信。Agora 提供的频道信息、计费、管理服务也都是基于 App ID。如果组织中有多个完全分开的应用程序，例如由不同的团队构建，则应使用不同的 App ID。如果应用程序需要相互通信，则应使用同一个 App ID。

### App Certificate

Agora 提供 App Certificate 用以生成 [动态密钥](#key)。

你可以在 [控制台](https://dashboard.agora.io) 获取  App Certificate，详细信息请参见 [获取 App Certificate](../../cn/Agora%20Platform/token.md)。

### <a name="key"></a>动态密钥

直接使用 App ID 非常方便，适用于应用程序的初始开发。然而，如果有人非法获取你的 App ID，那么他们可以在自己的客户端应用程序上使用你的 App ID，可以加入属于你并向你收费的通话。为了防止这种情况并保护应用程序，Agora 建议你使用动态密钥实现大规模生产应用程序。

动态密钥由服务器端代码通过 App Certificate 和其他密钥材料生成，且 App Certificate 在任何客户端代码中都无法访问，这使得动态密钥比静态 App ID 更安全。

动态密钥具有有效期，并包含客户端权限，例如不同的角色权限（[主播](#host)和[观众](#audience)）。

基于 SDK 版本，动态密钥可以是 Channel Key 或 Token，详细信息请查看 [校验用户权限](../../cn/Voice/token.md)。

### SD-RTN™

Agora 的音视频传输依赖于自建的 SD-RTN™ (Software Defined Real-time Network)，这是一种虚拟的、基于 UDP (User Datagram Protocol) 的网络架构，专为实时通信而设计。通过在互联网上不同的数据中心部署彼此协同工作的软件网络单元，Agora 添加了一个虚拟层。为确保传输的稳定性和低延迟，特别是在弱网环境下，SD-RTN™ 根据以下节点条件实时自动分配最优路径：

- 传输状态
- 负载条件
- 与用户的距离
- 响应时间

## SDK 核心概念

Agora SDK 通过 API 方法和事件回调实现音视频通话/[直播](#live)的各种功能。

- 方法：客户端调用 API 方法实现 Agora SDK 提供的功能。
- 回调：表示某些事件发生后，SDK 给客户端进行的反馈，包括两种事件: 本地事件和远端事件。

远端事件回调是远端用户的事件回调，通过 UDP 通道传输，并非 100％ 可靠。

有关 API 方法和回调的详细信息，请查看适用于 [Android](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/index.html)，[iOS/macOS](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/index.html)，[Web](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/index.html) 和[ Windows](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/index.html) 的 API 参考文档。

### 频道

我们把一个 App 比作一栋大楼的话，频道就好比大楼里面的一个房间。频道是由你调用 API 时在客户端创建，第一个用户加入时自动创建频道，最后一个用户离开时频道会自动销毁，无需维护。

加入频道时，用户需要提供 [App ID](#appid) 或者[动态密钥](#key)作为进入凭证，类似于进入房间需要正确的钥匙或者门卡。


### 频道模式

Agora SDK 将根据频道模式应用不同的优化方法。同一频道中的用户必须使用相同的频道模式。

目前我们有三种频道模式：


| 频道模式 | 描述| 
| ---------------- | ---------------- | 
| 通信     | 用于一对一通话或群组通话，频道中的所有用户都可以自由通话。 | 
| 直播     | 在直播模式的频道中，用户有两种角色：主播和观众。主播能够发送和接收音视频，观众不能发送，只能接收音视频。| 
| 游戏     | 频道中的任何用户都可以自由通话。 此模式默认使用具有低功耗和低码特率的编解码器。| 


> 游戏模式仅可用于 Agora Gaming SDK 。

### <a name="username"></a>用户名

在加入频道时需要传入用户名用于标识频道中的用户。同一频道中的每个用户都应具有唯一的用户名。

Agora 支持两种数据类型的用户名，整型（UID）和字符串类型（User Account）。你可以根据需要选择一种类型，确保频道中所有用户使用相同类型的用户名即可。

### 流

通常我们提到流或者 Stream 的时候指的是一个包含音视频数据的对象。在通话和直播中，用户可以[发布](#pub)本地的音视频流，[订阅](#sub)其他用户的音视频流。

### <a name = "pub"></a>发布

用户加入频道之后，可以向频道内的其他用户发送本地采集的音视频数据，即发布流。在直播模式下，只有[主播](#host)可以发布。

### <a name = "sub"></a>订阅

用户加入频道后，可以接收频道内的其他用户发布的音视频流，即订阅流。

### <a name ="dual"></a>双流模式

双流指视频大流和视频小流。发布端可以开启双流模式，同时发送大流和小流。订阅端可以根据自己的网络情况选择接收大流或小流。

大流和小流是一个相对的概念，通常小流占用的带宽会低于大流，适用于网络较差的场景。


| 视频类型 | 描述 | 
| ---------------- | ---------------- | 
| 大流     | 高分辨率、高码率的视频流。     | 
| 小流     | 低分辨率、低码率的视频流。     | 


### 流回退

在网络环境较差的情况下，发布者/订阅者可以开启回退以发送/接收视频小流或仅音频流。只有开启[双流模式](#dual)时，回退设置才会生效。

## <a name ="live"></a>直播核心概念

直播是指通过应用程序和互联网直播现场表演，观看者称为观众，表演者则称为主播。

Agora 产品可在任何应用程序上轻松实现直播功能：

- 创建直播，设置频道并以主播身份进入频道。
- 观看直播，以观众身份进入主播创建的频道。

### <a name ="host"></a>主播

直播频道中可以发送和接收流的用户。

### <a name ="audience"></a>观众

直播频道中仅能接收流的用户。观众只能接收，不能发送。

### 连麦

直播场景里，频道内的观众可以切换角色为主播，与频道内现有主播进行互动，称之为连麦。

### CDN 直播推流

将直播流发布到 CDN（Content Delivery Network）的过程称为 CDN 直播推流，用户可以通过 Web 浏览器观看直播。

### 转码

在推流到 CDN 过程中，当频道中有多个主播时，通常会涉及到转码。

在推流到 CDN 过程中，发送到 SD-RTN™ 的音视频流从 UDP 协议被转换成 RTMP（Real-Time Messaging Protocol）协议。如果有多个主播，就需要通过转码在协议转换之前将多个直播流组合成单个流，并设置这个流的音视频属性和合图布局。

> 我们建议不要在单主播的情况下使用转码。
