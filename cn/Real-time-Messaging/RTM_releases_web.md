
---
title: 发版说明
description: 
platform: Web
updatedAt: Tue May 21 2019 08:12:52 GMT+0800 (CST)
---
# 发版说明
## 简介

Agora RTM SDK 提供了稳定可靠、低延时、高并发的全球消息云服务，帮助你快速构建实时通信场景,  可实现消息通道、呼叫、聊天、状态同步等功能。点击 [实时消息产品概述](../../cn/Real-time-Messaging/RTM_product.md) 了解更多详情。

## 0.9.1 版

该版本于 2019 年 5 月 20 日发布。

### 新增功能

本版本增加了呼叫邀请功能。结合音视频一对一或一对多通话场景，你可以创建、发送、取消、接受或拒绝一个呼叫邀请。

### 性能改进

-   本地用户发送的频道消息/点对点消息/进频道通知均不出现在当前用户的 API 回调中。
-   用户名 `uid` 允许以空格开头。

### API 变更

### 新增

- [createLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#createlocalinvitation)：创建一个呼叫邀请
- [LocalInvitation.cancel](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send): （主叫）取消呼叫邀请
- [LocalInvitation.send](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send)： （主叫）发送呼叫邀请
- [LocalInvitationState](../../cn/Real-time-Messaging/RTM_releases_web.md)：返回给主叫的呼叫邀请状态码
- [LocalInvitationFailureReason](../../cn/Real-time-Messaging/RTM_releases_web.md)：返回给主叫的呼叫邀请失败原因。
- [RemoteInvitationReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmclientevents.html#remoteinvitationreceived): 收到来自对端的呼叫邀请回调
- [RemoteInvitation.accept](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept):  （被叫）接受呼叫邀请
- [RemoteInvitation.refuse](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#refuse)：（被叫）拒绝呼叫邀请
- [RemoteInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/enums/remoteinvitationstate.html)：返回给被叫的呼叫邀请状态码
- [RemoteInvitationFailureReason](../../cn/Real-time-Messaging/RTM_releases_web.md)：返回给被叫的呼叫邀请失败原因。


#### 重命名

-   `RtmClient` 的事件 `ConnectionStateChange` 更名为 [ConnectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmclientevents.html#connectionstatechanged) 。

#### 删除

-   `RtmChannel`  `getId` 方法，改用 [channelId(/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#channelid) 代替。


## 0.9.0 版

该版本于 2019 年 2 月 4 日发布。

首次发布。

### 主要功能

- 发送或接收点对点消息。
- 加入或离开频道。
- 发送或接收频道消息。

