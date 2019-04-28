
---
title: 发版说明
description: 
platform: Linux Java
updatedAt: Sun Apr 28 2019 06:03:14 GMT+0800 (CST)
---
# 发版说明
## 简介

作为声网信令 SDK 的升级换代产品，Agora RTM SDK 提供了更为简洁的逻辑实现和更为稳定的消息传送机制，帮助开发者快速搭建实时消息应用场景。

### 优势

- 支持发送点对点消息和频道消息。
- 支持一对一或一对多音视频通话的呼叫邀请。 
- 支持多实例，允许在一个线程中调用多个 RTM 服务。 
- 对错误码进行分类方便快速定位问题。

更多产品功能及性能介绍，详见： [产品概述](../../cn/Real-time-Messaging/RTM_product.md) 。

## 0.9.1 版

该版本于 2019 年 4 月 4 日发布。

### 新增功能

本版本增加了呼叫邀请功能。结合音视频一对一或一对多通话场景，你可以创建、发送、取消、接受或拒绝一个呼叫邀请。

### 性能改进

- 优化了对象关系帮助开发者快速搭建应用。
- 重命名部分接口以遵循 Java 命名规范。
- 删除状态 `ChannelMessageState` 和 `PeerMessageState` 以简化发送频道消息和点对点消息流程。以错误码 `ChannelMessageError` 和 `PeerMessageError` 代替。
- 删除用于监听消息状态的 `IStateListener` 类。改用 `ResultCallback` 类返回成功和错误回调。

### API 变更

#### 新增

- [LocalInvitation.setContent()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a4cec28ff6d356242329b1034c7531445): 用于主叫设置发出的呼叫邀请内容。
- [LocalInvitation.getContent()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a97294ce1b9b591f9d93e497869b1ad90): 用于主叫获取发出的呼叫邀请内容。 
- [LocalInvitation.getResponse()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a268c738458538a266d440b0e281328ee): 用于主叫获取被叫对发出呼叫邀请的响应内容。
- [LocalInvitation.getState()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a59608fbac8050f17ec0f855f28598d20): 用于主叫获取发出的呼叫邀请的状态。 
- [RemoteInvitation.getCallerId()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#ae38c5740aa9edb09749f0febb2663926): 用于被叫获取主叫的用户 ID。
- [RemoteInvitation.getContent()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#aeca3b3e981c69c44c7a618d4fdfb3b87): 用于被叫获取主叫设置的呼叫邀请内容。
- [RemoteInvitation.setResponse()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a229b8cf773eaa0e79b0d67815fd6b6f1): 用于被叫设置呼叫邀请响应。 
- [RemoteInvitation.getResponse()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a12a70e7d8a77eee21d37bbb65b2f9d3e): 用于被叫获取设置的呼叫邀请响应。
- [RemoteInvitation.getState()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#af77a4afabb19ff1468edf29720361a0f): 用于被叫获取收到的呼叫邀请的状态。
- [RtmCallEventListener.onLocalInvitationReceivedByPeer()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a24e1cb71d3e752963da49bdf91847788): 被叫收到呼叫邀请回调。 
- [RtmCallEventListener.onLocalInvitationAccepted()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a4dece02a62a187a66c2415fecf6b75dc): 被叫已接受呼叫邀请回调。 
- [RtmCallEventListener.onLocalInvitationRefused()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6224643c400268d356cb5d489825bdd0): 被叫已拒绝呼叫邀请回调。 
- [RtmCallEventListener.onLocalInvitationCanceled()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#ae3164e81772cd4d6171165b1705adcaa): 呼叫邀请已取消回调。 
- [RtmCallEventListener.onLocalInvitationFailure()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#acfefb97eaca497cbd71a0c1cbf5067b0): 发出的呼叫邀请进程失败回调。 
- [RtmCallEventListener.onRemoteInvitationReceived()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a8d01498a993c4016aa45ccb9bf4e9097): 收到呼叫邀请回调。 
- [RtmCallEventListener.onRemoteInvitationAccepted()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a81d9d3de89d08c41408d8a94c8309d29): 已接受呼叫邀请回调。 
- [RtmCallEventListener.onRemoteInvitationRefused()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a7a21eaa9ff49bcf39e3c49b94f6e6ac7): 已拒绝呼叫邀请回调。 
- [RtmCallEventListener.onRemoteInvitationCanceled()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a9d0409c87455d4d2b1315f67a5f7aa12): 主叫已取消呼叫邀请回调。 
- [RtmCallEventListener.onRemoteInvitationFailure()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6f9f2bbbfbcb0a766c6f1b2e4a8314a1): 发给被叫的呼叫邀请进程失败回调。 
- [RtmCallManager.setEventListener()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a934eee4922584707a1a7ef9ac6999cf2): 设置 `RtmCallManager` 实例的事件监听。
- [RtmCallManager.createLocalInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a1756dca077267acaa407c6901daa2248): 创建呼叫邀请。 
- [RtmCallManager.sendLocalInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#af899697061305ca840e829b92c78e353): 向指定用户发送呼叫邀请。 
- [RtmCallManager.acceptRemoteInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f6f97c84e426e2fbd8a5dda71e2fc6c): 用于被叫接受呼叫邀请。 
- [RtmCallManager.refuseRemoteInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a2ce4af944183976d18c055816f756bf6): 用于被叫拒绝呼叫邀请。 
- [RtmCallManager.cancelLocalInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f03bfe1cfd6987fbc7b5a4dc484f564): 用于主叫取消呼叫邀请。 
- [RtmStatusCode#LocalInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_state.html): 返回给主叫的呼叫邀请状态码。
- [RtmStatusCode#RemoteInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_remote_invitation_state.html): 返回给被叫的呼叫邀请状态码。 
- [RtmStatusCode#LocalInvitationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_error.html): 返回给主叫的呼叫邀请错误码。 
- [RtmStatusCode#RemoteInvitationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_remote_invitation_error.html): 返回给主叫的呼叫邀请错误码。 
- [RtmStatusCode#InvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_invitation_api_call_error.html): 呼叫邀请相关 API 方法调用的错误码。 
- [ConnectionChangeReason#CONNECTION_CHANGE_REASON_REMOTE_LOGIN](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_connection_change_reason.html): 另一个实例已用相同的用户 ID 登录 Agora RTM 系统。

#### 重命名

- 将用于释放 `RtmClient` 实例的所有资源的 `RtmClient.destroy()` 方法更名为 [RtmClient.release()](../../API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html.md) 。
- 将 `IResultCallback` 类更名为 [ResultCallback](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html) 类。

#### 删除

- 从 [PeerMessageError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_peer_message_error.html) 删除 `PEER_MESSAGE_RECEIVED_BY_SERVER` ，改用错误码  `PEER_MESSAGE_ERR_OK` 。
- 从 [ChannelMessageError](../../API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_channel_message_error.html.md) 删除 `CHANNEL_MESSAGE_RECEIVED_BY_SERVER`  ，改用错误码 `CHANNEL_MESSAGE_OK` 。
- 删除 `PeerMessageState` 接口，改用 [PeerMessageError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_peer_message_error.html) 。
- 删除 `ChannelMessageState` 接口，改用 [ChannelMessageError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_channel_message_error.html) 返回频道消息错误码。
- 删除用于监听消息状态的 `IStateListener` 类，改用 [ResultCallback](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html) 类监听点对点消息和频道消息结果。
  - 成功：SDK 返回 [ResultCallback.onSuccess()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5) 回调。
  - 失败：SDK 返回 [ResultCallback.onFailure()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) 回调以及相应的错误码

## 0.9.0 版

该版本于 2019 年 2 月 4 日发布。

首次发布。

关键功能

- 发送或接收点对点消息。
- 加入或离开频道。
- 发送或接收频道消息。
