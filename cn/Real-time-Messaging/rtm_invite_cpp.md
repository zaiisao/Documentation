
---
title: 呼叫邀请
description: 
platform: Windows CPP,Linux CPP
updatedAt: Fri Jul 17 2020 06:45:32 GMT+0800 (CST)
---
# 呼叫邀请
## 概述

Agora RTM SDK 提供的呼叫邀请功能是基于 RTM 点对点消息的功能封装的类似 SIP 的协议，用于呼叫控制。呼叫邀请功能覆盖了通用呼叫场景中的以下行为：

- 主叫发送呼叫邀请；
- 主叫取消呼叫邀请；
- 被叫接受收到的呼叫邀请；
- 被叫拒绝收到的呼叫邀请。

Agora RTM SDK 还支持：

- 主叫在发送邀请时提供附加信息，比如：媒体频道名（channelId）和 content；
- 被叫在接受或拒绝邀请时提供相应内容（response）；
- 单个主叫同时发送多个呼叫邀请（支持向同一或多个被叫同时发送多个呼叫邀请）。

<div class="alert note">Agora RTM SDK 提供的呼叫邀请功能仅仅实现了通用呼叫邀请的基本的控制逻辑。Agora RTM SDK 不会处理邀请接通之后的动作（比如加入指定媒体频道），用户需要根据自己的业务逻辑自行实现。</div>

## 应用场景

- APP 的呼叫邀请的响铃功能；
- 邀请对方结合 Agora Native SDK 进行屏幕共享；

- 两个用户之间同时或先后发起视频呼叫和白板共享功能；
- 需要同步状态的呼叫场景

## 呼叫邀请生命周期定义

Agora RTM SDK 在一个呼叫邀请过程中引入了 ILocalCallInvitation 和 IRemoteCallInvitation 的概念。你可以将这两个呼叫邀请对象理解为同一个呼叫邀请的两种不同形式。

### 生命周期的开始

- ILocalCallInvitation 由主叫通过显式调用 [createLocalCallInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_manager.html#a71f1d4f359e62f0a2ed83d19a69dd095) 方法创建，供主叫指定被叫、设置自定义内容，或查询呼叫邀请状态。
- IRemoteCallInvitation 在被叫收到呼叫邀请时由 SDK 创建并通过 onRemoteInvitationReceived 回调返回，供被叫设置相应内容、查询主叫 ID，或查询呼叫邀请状态。

### 生命周期的结束

ILocalCallInvitation 的生命周期在主叫收到以下回调时结束，需主动调用 [release](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_local_call_invitation.html#a090ae08f677e799deef2ffb17fcf6944) 释放：

- [onLocalInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_event_handler.html#ac97269b4f483946456a3d0f9aabaf1e1)：主叫已成功取消呼叫邀请。
- [onLocalInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_event_handler.html#a55e4dcc83699c4d2a17521e0c0dfdf30)：被叫已接受呼叫邀请。
- [onLocalInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_event_handler.html#a5c87e054ec1c004fd11e721033a2359c)：被叫已拒绝呼叫邀请。
- [onLocalInvittionFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_event_handler.html#a34e366966b97603289288f229c83e2af)：呼叫邀请过程已开始但是以失败告终。该回调通过错误码 [LOCAL_INVITATION_ERR_CODE](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/namespaceagora_1_1rtm.html#aa83fb27d8066fb5aa6e591e5964edadc) 概括了失败的主要原因：
  - `LOCAL_INVITATION_ERR_PEER_OFFLINE`：被叫不在线。
  - `LOCAL_INVITATION_ERR_PEER_NO_RESPONSE`：被叫无响应（呼叫邀请发出 30 秒后仍未收到被叫 ACK 响应）
  - `LOCAL_INVITATION_ERR_INVITATION_EXPIRE`：呼叫邀请过期（被叫收到呼叫邀请 60 秒后呼叫邀请未被取消、接受，或拒绝）

IRemoteCallInvitation 的生命周期在被叫收到以下回调时结束，需调用 [release](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_remote_call_invitation.html#a3e2d65de7cf67a91104718855a03108d) 主动释放：

- [onRemoteInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_event_handler.html#a094f89aaa7d7eeb928fe90f537a6680b)：主叫已成功取消呼叫邀请。
- [onRemoteInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_event_handler.html#a3edc69d558bead97f882de0147df449e)：接受呼叫邀请成功。
- [onRemoteInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_event_handler.html#aaf8ce8c0810625efd29e61bad19a98ad)：拒绝呼叫邀请成功。
- [onRemoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_event_handler.html#a25af8d459354093f18580a41c64bbbf4)：呼叫邀请过程以失败告终。该回调通过错误码 [	REMOTE_INVITATION_ERR_CODE](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/namespaceagora_1_1rtm.html#aedb89e7145338eb7a1b05df69f31e26f) 概括了失败的主要原因：
  - `REMOTE_INVITATION_ERR_PEER_OFFLINE`：接受呼叫邀请时主叫不在线。
  - `REMOTE_INVITATION_ERR_ACCEPT_FAILURE`：被叫接受呼叫邀请后，主叫未响应。
  - `REMOTE_INVITATION_ERR_INVITATION_EXPIRE`：呼叫邀请过期（被叫收到呼叫邀请 60 秒后呼叫邀请未被取消、接受，或拒绝）

> 被叫接受来自主叫的呼叫邀请是一个三次握手的过程：
>
> - 一次握手：主叫调用 sendLocalInvitation 发送呼叫邀请，被叫收到 onRemoteInvitationReceived 回调；
> - 二次握手：被叫调用 [acceptRemoteinvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_manager.html#ac97ae6e4f8f7f3add980fdd15f9c47ba) 方法接受呼叫邀请，主叫收到被叫已接受呼叫邀请的信息；
> - 三次握手：被叫确认主叫已经收到了被叫接受邀请的信息，呼叫邀请成功。

## 通用行为限定 

了解了呼叫邀请生命周期定义后，我们就能够理解 Agora RTM SDK 对于呼叫邀请的行为限定：所有的呼叫邀请操作都应在呼叫邀请的生命周期内进行。

在主叫显式调用 [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_manager.html#a5d1ea87573a3e65ded0886f42aefd445) 或 [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_manager.html#ab92d7383de69eff8d8dbcc19114751bc) 方法发送或取消呼叫邀请时，或当被叫显式调用 [acceptRemoteinvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_manager.html#ac97ae6e4f8f7f3add980fdd15f9c47ba) 或 [refuseRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_call_manager.html#ab6ca977d3075800ec014fa536622f2ff) 方法接受或拒绝呼叫邀请时，SDK 会对 APP 行为进行检查返回相应的错误码 [INVITATION_API_CALL_ERR_CODE](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/namespaceagora_1_1rtm.html#a699db038c2078a6cc2d80bd3b0a39acb) 错误码：

| 回调                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `INVITATION_API_CALL_ERR_NOT_STARTED` | 呼叫邀请未开始。错误情况包括：在 sendLocalInvitation 之前调用了 cancelLocalInvitation。 |
| `INVITATION_API_CALL_ERR_ALREADY_END` | 呼叫邀请结束后又调用了 send、cancel、accept，或 refuse。     |
| `INVITATION_API_CALL_ERR_ALREADY_ACCEPT` | 被叫重复调用 accept 方法。                                   |
| `INVITATION_API_CALL_ERR_ALREADY_SENT` | 主叫重复调用 send 方法发送呼叫邀请。                         |

## 状态流转图

呼叫邀请中，主叫可以通过 [ILocalCallInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_local_call_invitation.html) 对象的 [getState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_local_call_invitation.html#adc3a57b448247e0db2f595dd02314634) 方法查询当前呼叫邀请的有关状态；被叫可以通过 SDK 返回的 [IRemoteCallInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_remote_call_invitation.html) 对象的 [getState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_remote_call_invitation.html#a225c2d37384c45be0b1305cb68f55e0d) 方法查询当前呼叫邀请的相关状态。

### LocalInvitationState 

下图描述了与主叫相关的呼叫邀请状态流转图：

![](https://web-cdn.agora.io/docs-files/1582270646018)

### RemoteInvitationState 

下图描述了与被叫相关的呼叫邀请状态流转图：

![](https://web-cdn.agora.io/docs-files/1582270656158)

## API 时序图

### 取消已发送呼叫邀请

![](https://web-cdn.agora.io/docs-files/1565755588740)

### 接受／拒绝呼叫邀请

![](https://web-cdn.agora.io/docs-files/1565755596730)

## 注意事项及限制条件

- 主叫设置的呼叫邀请 content 的字符串长度：8 KB，格式为 UTF-8。
- 被叫设置的呼叫邀请响应 response 的字符串长度：8 KB，格式为 UTF-8。
- 呼叫邀请的 channel ID 仅用于与老信令互通时设置。设置的 channel ID 必须与老信令 SDK 设置相同才能实现互通。字符串长度：64 字节，格式为 UTF-8。



## API 参考

详见：[呼叫邀请管理](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/index.html#callinvitation)。

## 示例项目

我们在 Github 提供一个开源的[示例项目](https://github.com/AgoraIO-Usecase/Video-Calling)，你也可以前往下载体验并参考源代码。
