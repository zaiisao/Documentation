
---
title: 语聊房
description: 
platform: Android
updatedAt: Mon Feb 10 2020 02:43:22 GMT+0800 (CST)
---
# 语聊房
## 场景介绍

一个典型的语聊房里，有一个房主，N 个观众。房间里所有观众都能听到房主的声音，也可以自由上麦、下麦。房主可以邀请观众上麦，或进行下麦、禁麦、解麦、封麦、解封等操作。同时，所有用户都能看到麦位的实时变动。该场景在语音社交行业内应用广泛，尤其适用于在线 KTV、语音电台等场景。

下图红框区域为麦位示意图。语聊房内所有用户都可以在该区域看到麦位的实时变动，包括：某个麦位是否有主播上麦，主播是否为禁言状态，共有几个主播上麦等。

![](https://web-cdn.agora.io/docs-files/1578301622713)

### 功能列表

<style> table th:first-of-type {     width: 90px; } </style>

| 功能 | 描述 | 
| ---------------- | ---------------- | 
| 实时音频      | 超低延时下，观众实时接收房主的音频流，保证语聊房的社交氛围。      | 
| 互动连麦      | 	房主邀请或观众请求上麦，观众上麦后成为连麦主播，频道所有用户都能听到房主和连麦主播的声音。|
|  麦位控制     | 房主对观众进行上麦、下麦、禁麦、解麦、封麦、解封等操作，观众可以实时看到每个麦位及各麦位上观众的状态。其中：<ul><li>上麦：房主邀请观众上麦。观众上麦后成为连麦主播，可以和房主实时互动；</li><li>下麦：房主将连麦主播恢复为普通观众；</li><li>禁麦：房主不允许连麦主播发言；<li>解禁：房主恢复连麦主播的发言权限；</li><li>封麦：房主将某个麦位封掉，不允许观众在该麦位上麦；</li><li>解封：房主恢复已封的麦位，观众可以在该麦位上麦。</li></ul>|
| 实时消息     | 房间内的房主、连麦主播和观众使用文字消息实时交流；观众还可以通过实时消息给主播送礼物，增加互动气氛。|
| 用户管理     | 维护房间成员列表、用户昵称等。|
| 混音             | 房主在说话的同时播放背景音乐，语聊房内所有观众都能听到，可以烘托主题氛围。|
| 变声             | 通过变声，房主和连麦主播可以让自己的声音更有特色、符合人设、增加互动趣味。|

### Demo 体验

Agora 为语聊房提供如下平台的 Demo，扫描下方二维码即可体验。

![](https://web-cdn.agora.io/docs-files/1578297683177)

## 技术方案

Agora 使用 Agora RTC SDK 与 Agora RTM SDK 共同搭建语聊房场景。其中：

- 有且仅有一个房主。
- 可以有多个观众。
- 观众成功上麦后，成为连麦主播。其余观众则还是普通观众。
- Agora 目前仅支持最多一个房主+十六个连麦主播实时互动。

### 技术架构图

- 所有用户分别加入 RTC 和 RTM 频道。房主和观众可以自由上麦或下麦；房主还可以对麦位进行封麦、解封等操作。整个过程中，RTM SDK 会根据麦位状态同步更新频道属性。

![](https://web-cdn.agora.io/docs-files/1578297738584)

- 房主通过 RTM SDK 发送指令，要求观众上麦或下麦，观众收到指令后，通过 RTC SDK 进行上、下麦操作，同时 RTM SDK 根据麦位状态同步更新频道属性。

![](https://web-cdn.agora.io/docs-files/1578297759490)

- 房主、连麦主播和观众可以通过 RTM SDK 发送实时消息、赠送礼物，通过 RTC SDK 实现音频相关控制，如本地变声、伴奏、静音等，为整个场景提供更多玩法和乐趣。

![](https://web-cdn.agora.io/docs-files/1578297822869)

### 方案优势

1、超低延时的实时音视频体验
- 端到端平均延时低至 200-300 ms ，最低 50 ms；
- 频道成员连麦时间几乎无感知。

2、可靠的实时消息连接
- RTM SDK 提供精准的麦位控制体验；
- 音视频与文字消息实时同步。

3、丰富的变声效果
- 提供 9 种变声音效、10 种美声效果，增加互动趣味。

## 实现方案

该章节展示语聊房相关功能的 API 时序图及示例代码。你可以参考该部分文档，结合我们在 GitHub 上的开源示例项目 [Chatroom](https://github.com/AgoraIO-Usecase/Chatroom/tree/master/Android)，在项目中快速实现相关功能。

### 集成指引

根据下表的链接，分别下载对应平台的 Agora RTC SDK 和 Agora RTM SDK，并参考集成文档将 SDK 集成到你的项目中。

| 产品 | 产品概述 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- | --------------- |
| RTC SDK      | [音频互动直播](../../cn/Real-time-Messaging/product_live_audio.md)      | [音频互动直播 SDK 下载](https://docs.agora.io/cn/Audio%20Broadcast/downloads)      | [实现互动直播](../../cn/Real-time-Messaging/start_live_android.md) |
| RTM SDK     | [实时消息](../../cn/Real-time-Messaging/product_rtm.md) | [实时消息 SDK 下载](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [发送点对点消息和频道消息](../../cn/Real-time-Messaging/messaging_android.md) |

### 核心 API 时序图

- 初始化及加入频道

![](https://web-cdn.agora.io/docs-files/1578298871815)

- 麦位控制

![](https://web-cdn.agora.io/docs-files/1578298898752)

其中，详细的 API 定义及功能描述详见下表：

- RTC SDK

	| API | 描述 |
	| ---------------- | ---------------- | 
	| [joinChannel](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c)      | 加入 RTC 频道。加入后，可以进行实时语音聊天互动。     | 
	| [setClientRole](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa2affa28a23d44d18b6889fba03f47ec)      | 设置用户角色为主播或观众，其中：<ul><li>主播可以接收、发送音频流；</li><li>观众只可以接收音频流。</li></ul>你可以在加入频道前设置用户角色；也可以在加入频道后调用该方法切换用户角色。该方法可结合 RTM 指令实现上麦、下麦功能。 |
	| [muteLocalAudioStream](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a838a04b744e6fb53bd1548d30bff1302)     | 停止发送本地音频流。该方法可用于实现本地静音；结合 RTM 指令也可以实现观众禁言或解禁功能。 |

- RTM SDK

	| API | 描述 | 
	| ---------------- | ---------------- | 
	| [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a)      | 登录 RTM 系统。     | 
 | [createChannel](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a)     | 创建 RTM 频道。 |
 | [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40)     | 加入 RTM 频道。加入后，可以发送频道消息，实现实时消息功能。 |
 | [getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a81f14a747a4012815ab4ba8d9e480fb6)     | 获取频道属性。频道属性存储频道信息，包含频道名、频道内的麦位信息、各麦位对应的用户，及麦位状态。 |
 | [sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9)     | 发送点对点消息。主播使用该方法可以实现发送指令等功能。 |
 | [addOrUpdateChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a997a31e6bfe1edc9b6ef58a931ef3f23)     | 添加或更新频道属性。当麦位顺序、麦位状态或麦位-用户对应关系发生更新时，该方法将更新同步到频道属性内，通知频道内所有用户。 |
 
### 附加功能
 
 你还可以根据场景需求，参考如下进阶功能指南，在项目中实现相关功能。
 
 - [播放音效或混音](../../cn/Real-time-Messaging/audio_effect_mixing_android.md)
 - [变声与混响](../../cn/Real-time-Messaging/voice_changer_android.md)

### 开源示例代码

我们在 GitHub 上提供了 [Chatroom 的开源示例项目](https://github.com/AgoraIO-Usecase/Chatroom/tree/master/Android)，你可以前往下载，或查看其中的源代码。其中 manager 和 model 层体现了该场景的核心业务逻辑。



