
---
title: Agora 平台概述
description: 
platform: All Platforms
updatedAt: Fri Mar 27 2020 04:31:17 GMT+0800 (CST)
---
# Agora 平台概述
声网 Agora 为开发者提供实时音视频 API，只需集成 Agora SDK，即可快速在应用内构建多种实时互动场景。

## SDK 产品

开发者集成 Agora SDK 后，即可以调用不同 API，实现不同场景的实时互动。

| Agora SDK      | 实现功能             | 描述                                                         |
| -------- | -------------------- | ------------------------------------------------------------ |
| 语音 SDK  | [语音通话](../../cn/Voice/product_voice.md)<br>[音频互动直播](../../cn/Audio%20Broadcast/product_live_audio.md) | SDK 包体积较小，只针对纯音频场景。            |
| 视频 SDK  | [视频通话](../../cn/Video/product_video.md)<br>[视频互动直播](../../cn/Interactive%20Broadcast/product_live.md) | 同时包含语音和视频功能。                                     |
| 游戏 SDK  | [互动游戏](../../cn/Interactive%20Gaming/product_gaming.md)            | 专门针对游戏开发者提供，包体积最小 1 M 左右。                |
| RTM SDK  | [实时消息](../../cn/Real-time-Messaging/product_rtm.md)            | 提供稳定可靠、低延时、高并发的全球消息云服务，快速构建实时场景。                |
| 录制插件 | [本地服务端录制](../../cn/Recording/product_recording.md)<br>[云端录制](../../cn/cloud-recording/product_cloud_recording.md)             | 可以将语音聊天、视频聊天或直播的内容储存下来，提供给更多人在方便的时间观看。 |

## 自建基础设施

SD-RTN™（Software Defined Real-time Network）软件定义实时网，这是声网自建的底层实时传输网络，实际上，所有通过声网 SDK 接入的实时音视频数据都是通过 SD-RTN™ 传输和调度。这也是全球唯一一个专门针对实时传输设计的基础设施。目前，声网在全球部署近 200 个数据中心，通过智能动态路由算法，确保全球范围内的毫秒级超低延迟传输，保证技术服务高可用。

| 特性                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| 全球网络覆盖        | <li>覆盖全球 200+ 国家和地区<li>国内数十家中小运营商全覆盖           |
| 接入能力            | <li>多智能终端接入<li>单频道可支持百万人同时在线             |
| QoS 能力增强        | <li>提前预防网络拥塞<li>弱网抗丢包保证                       |
| 基于 QoS 的动态路由 | <li>网络资源综合评估<li>QoS 最优路径保证                     |
| 技术服务 SLA 保障   | <li>7 &times; 24 服务支持保障，包含工单/IM/论坛<li>一对一 VIP 客户服务支撑 |
| 全球网络可靠性   | <li>99.999% 高可用<li>隐藏核心业务，如防 DDOS |
| 全平台互通          | <li>支持 6000+ 终端设备适配<li>支持 Chrome、Safari、Firefox 等主流浏览器<li>支持 iOS、Android、Web、Windows、MacOS、Electron、Linux、CoCos、Unity、小程序等平台 |
| 底层协议            | 基于 UDP 协议优化多个私有协议                                |
| 抗丢包优化          | 独创弱网优化抗丢包机制算法，音频 70% 丢包可用                |

## 自研音视频编解码

声网是全球业内唯一使用自研音视频编解码的 RTC 技术服务商。因此，在音视频质量上，有一些独特的优势和特点。

### 音频

- 高保真、3D 环绕立体声体验
- 48 kHz 全频带采集：高度还原原声
- 基于机器学习的 3A 算法：回声消除、自动增益、噪声抑制
- 听觉增强：双声道、全景声、听声辨位、混音、混响特效、耳返、变声

### 视频

- 沉浸式视觉体验
- 持续性网络探测：编码前中后对网络进行探测，对网络友好
- 动态网络流控：保持网络带宽资源动态平衡
- 高效抗丢包编码产品：编码算法优化，平滑视频传输，防止网络冲击
- 丢包补偿：自动修复内容缺失，确保视觉体验
- 视觉增强：基于机器学习的美颜功能

## 开发者工具和支持

1. [开发者中心](https://docs.agora.io/cn)提供集成和使用 Agora 产品所需的文档、SDK 和 Sample Code 下载。
2. [控制台](https://console.agora.io/stat) 提供用量统计、项目管理、权限管理、质量追踪、付费等功能，详情见 [控制台操作指南](../../cn/Interactive%20Broadcast/dashboard.md)。
3. Agora [GitHub 官方](https://github.com/AgoraIO) 和 [GitHub 社区](https://github.com/AgoraIO-Community) 提供丰富的开源示例程序和场景化解决方案，也可以通过[开发者中心](https://docs.agora.io/cn/Agora%20Platform/sampleapps)直接获得。
6. 开发者支持与服务保证 5 &times; 8，集成问题可提交[开发者社区](https://rtcdeveloper.com)提问，售后质量问题可[提交工单](https://console.agora.io/show-ticket-submission)。
7. [水晶球](https://console.agora.io/analytics/call/search) Agora Analytics，为开发者提供的全周期通话质量监测、回溯和分析的解决方案，致力于帮助你及时发现问题、定位原因，并最终解决问题以提升用户体验。详见[水晶球概览](../../cn/Agora%20Platform/aa_guide.md)。
