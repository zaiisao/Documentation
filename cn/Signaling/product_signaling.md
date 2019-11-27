
---
title: 产品概述
description: 
platform: All Platforms
updatedAt: Mon Nov 25 2019 03:52:38 GMT+0800 (CST)
---
# 产品概述
Agora Signaling SDK 基于 TCP 协议，提供了稳定可靠的消息通道，帮助快速构建实时场景。

> 为了提供更加稳定可靠、弹性可扩展、全球化的实时消息服务，帮助快速构建实时互动体验，声网全新推出了实时消息 Agora RTM (Real-time Messaging) SDK。 Agora RTM SDK 将比 Agora Signaling SDK 支持更多功能和更丰富的场景应用，并在未来逐步替代信令。请点击 [产品概述](../../cn/Real-time-Messaging/RTM_product.md) 获取更多关于实时消息 SDK 的信息。如需升级到 Agora RTM SDK，请参考[信令与 RTM 功能对照表](https://docs.agora.io/cn/Real-time-Messaging/RTM_vs_signaling_android?platform=Android)。

## 功能概述

Agora Signaling SDK 能够实现以下功能：

-   点对点消息

-   频道消息

-   获取用户属性

-   获取频道属性

-   获取频道内用户列表、人数回调


## 适用场景

Agora Signlaing SDK 应用丰富，适用于以下实时场景：

<table>
  <tr>
    <th>行业</th>
    <th>适用场景</th>
  </tr>
  <tr>
    <td>直播</td>
    <td><li>弹幕<br><li>聊天室<br><li>礼物、点赞<br><li>直播间状态维护（频道内人数）<br><li>频道列表<br><li>权限控制（踢人、禁言、禁麦）</td>
  </tr>
  <tr>
    <td>社交</td>
    <td><li>私聊消息<br><li>群消息<br><li>语音/视频呼叫邀请指令</td>
  </tr>
  <tr>
    <td>教育</td>
    <td><li>班级群消息<br><li>私聊消息<br><li>白板<br><li>权限管理（奖励、上讲台、举手、点赞）</td>
  </tr>
  <tr>
    <td>游戏</td>
    <td>游戏同步</td>
  </tr>
  <tr>
    <td>IoT</td>
    <td>控制消息</td>
  </tr>
</table>


## 相关文档

Agora 为你提供了如下文档，方便在使用中查阅：

-   [发送点对点文本消息和频道文本消息](../../cn/Quickstart%20Guide/signal_android-1.md) 展示了如何从零开始完成 Agora Signaling SDK 录制 SDK 的部署及使用，包括环境搭建、集成方法、编译代码、演示录制等内容。

-   [信令 API](../../cn/API%20Reference/signal_android.md) 展示了使用 Agora Signaling SDK 过程中你可以调用的各 API，以及调用这些 API 能实现的功能、以及会收到的回调等内容。

## 防火墙与白名单

对于有外网访问限制的公司，在使用 Agora 信令服务之前，需要添加防火墙白名单。

### 防火墙端口

| 端口     | 白名单项目        |
| -------- | ----------------- |
| TCP 端口 | 1080；8001 - 8199；10000 - 10010；10100 - 10110 |
| UDP 端口 | 8180 - 8199       |

### 域名白名单

```
 .agora.io
 qoslbs.agoralab.co
 qos.agoralab.co
```



