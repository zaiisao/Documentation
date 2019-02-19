
---
title: 实现客户端连麦
description: 
platform: Web
updatedAt: Fri Sep 28 2018 19:57:58 GMT+0800 (CST)
---
# 实现客户端连麦
# 实现客户端连麦

本文描述如何在客户端实现连麦功能。

目前市面上大多数直播技术方案为: 将主播端音视频数据通过 RTMP 协议推流到 CDN 云端，但存在观众观看延时大、主播无法和观众互动等缺点。 使用 Agora Web SDK 的连麦功能，配合直播厂商或 Agora 服务器的推流服务，可以实现:

- 观众随时申请成为嘉宾与主播连麦
- 多个嘉宾同时和主播连麦、进行实时互动

连麦都是在客户端实现的，如有推流需求，你可以结合 [进阶：推流到 CDN](../../cn/Quickstart%20Guide/push_stream_web.md) 选择在服务器端推流还是客户端推流。本文以两人连麦场景为例。

### 准备工作

- 如果你想实现语音连麦，确保你已经根据 [入门: 实现语音直播](../../cn/Quickstart%20Guide/broadcast_audio_web.md) 实现语音直播功能。
- 如果你想实现音视频连麦，确保你已经根据 [入门: 实现视频直播](../../cn/Quickstart%20Guide/broadcast_video_web.md) 实现视频直播功能。

## 示例

1. 用户 A 调用 **加入 AgoraRTC 频道** \(`client.join`\) 加入频道，然后调用 **发布本地音视频流**\(`client.publish`\)，切换成主播角色。
2. 用户 B 调用 **加入 AgoraRTC 频道** \(`client.join`\)  加入频道，以默认的观众身份加入频道, 然后调用 **发布本地音视频流**\(`client.publish`\), 即可连麦；调用 **取消发布本地音视频流** \(`client.unpublish`\)，即可下麦。
