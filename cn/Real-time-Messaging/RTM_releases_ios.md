
---
title: 发版说明
description: 
platform: iOS,macOS
updatedAt: Sat Apr 27 2019 06:23:52 GMT+0800 (CST)
---
# 发版说明
## 简介

作为声网信令 SDK 的升级换代产品，Agora RTM SDK 提供了更为简洁的逻辑实现和更为稳定的消息传送机制，帮助开发者快速搭建实时消息应用场景。

### 优势

- 支持发送点对点消息和频道消息。
- 支持一对一或一对多音视频通话的呼叫邀请。 
- 支持多实例，允许在一个线程中调用多个 RTM 服务。 
- 对错误码进行分类方便快速定位问题。

## 0.9.1 版

该版本于 2019 年 4 月 4 日发布。

### 新增功能

本版本增加了呼叫邀请功能。结合音视频一对一或一对多通话场景，你可以创建、发送、取消、接受或拒绝一个呼叫邀请。

> 本版本不包含 `setLogFile` 和 `setLogFilter` 方法。所有的日志信息默认保存在 **~/Library/caches/agorasdk.log** 。

### 性能改进

- 优化了对象关系帮助开发者快速搭建应用。
- 简化了发送频道消息或点对点消息的流程。

### API 变更

#### 新增

- [initWithCalleeId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html#//api/name/initWithCalleeId:): 创建一个 `AgoraRtmLocalInvitation` 实例。

- [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html): 用于主叫读取的呼叫邀请对象。

- [localInvitationReceivedByPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationReceivedByPeer:): 被叫已接收到呼叫邀请回调。

- [localInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationAccepted:withResponse:): 被叫已接受呼叫邀请回调。

- [localInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationRefused:withResponse:): 被叫已拒绝呼叫邀请回调。

- [localInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationCanceled:): 已取消呼叫邀请回调。

- [localInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationFailure:errorCode:): 发出的呼叫邀请进程失败回调。

- [remoteInvitationReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationReceived:): 收到呼叫邀请回调。

- [remoteInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationAccepted:): 已接受呼叫邀请回调。

- [remoteInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationRefused:): 已拒绝呼叫邀请回调。

- [remoteInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationCanceled:): 主叫已取消呼叫邀请回调。

- [remoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationFailure:errorCode:): 发给被叫的呼叫邀请进程失败回调。

- [AgoraRtmCallDelegate](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html): 提供呼叫邀请相关回调。

- [sendLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:): 向指定用户发送呼叫邀请。

- [acceptRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/acceptRemoteInvitation:completion:): 用于被叫接受呼叫邀请。

- [refuseRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/refuseRemoteInvitation:completion:): 用于被叫拒绝呼叫邀请。

- [cancelLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:): 用于主叫取消呼叫邀请。

- [AgoraRtmLocalInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationState.html): 返回给主叫的呼叫邀请状态码。

- [AgoraRtmRemoteInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationState.html): 返回给被叫的呼叫邀请状态码。

- [AgoraRtmLocalInvitationErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationErrorCode.html): 返回给主叫的呼叫邀请错误码。

- [AgoraRtmRemoteInvitationErrorCode](https://docs.agora.io/en/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationErrorCode.html): 返回给主叫的呼叫邀请错误码。

- [AgoraRtmInvitationApiCallErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmInvitationApiCallErrorCode.html): 呼叫邀请相关 API 方法调用的错误码。

- [AgoraRtmConnectionChangeReasonRemoteLogin](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmConnectionChangeReason.html): 另一个实例已用相同的用户 ID 登录 Agora RTM 系统。

- [AgoraRtmSendPeerMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendPeerMessageErrorCode.html): 发送点对点消息相关错误码。 

- [AgoraRtmSendChannelMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendChannelMessageErrorCode.html): 发送频道消息相关错误码。 


#### 删除

- 删除常量 `AgoraRtmSendMessageErrorCode` ，改用 [AgoraRtmSendPeerMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendPeerMessageErrorCode.html)  和 [AgoraRtmSendChannelMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendChannelMessageErrorCode.html) 。

#### 修正

- 将 `channelDelegate` 设为 property 方便开发者设置。 
- 将参数 `rtmKit` 从 `AgoraRtmChannelDelegate` 下的回调中删除。
- 更改了通过 JSON 选项配制 SDK 的 [setParameters](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setParameters:) 方法的参数。
- 从 [AgoraRtmSendChannelMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendChannelMessageErrorCode.html) 删除`AgoraRtmSendChannelMessageStateReceivedByServer` 状态。
- 删除点对点消息状态 AgoraRtmSendPeerMessageState interface ，改用错误码 [AgoraRtmSendPeerMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendPeerMessageErrorCode.html) 。
- 删除频道消息状态 AgoraRtmSendChannelMessageState ，改用错误码 [AgoraRtmSendChannelMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendChannelMessageErrorCode.html) 。

## 0.9.0 版

该版本于 2019 年 2 月 4 日发布。

首次发布。

关键功能

- 发送或接收点对点消息。
- 加入或离开频道。
- 发送或接收频道消息。


