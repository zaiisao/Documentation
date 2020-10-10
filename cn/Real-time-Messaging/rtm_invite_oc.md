
---
title: 呼叫邀请
description: 
platform: iOS
updatedAt: Fri Jul 17 2020 06:41:45 GMT+0800 (CST)
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

Agora RTM SDK 在一个呼叫邀请过程中引入了 AgoraRtmLocalInvitation 和 AgoraRtmRemoteInvitation 的概念。你可以将这两个呼叫邀请对象理解为同一个呼叫邀请的两种不同形式。

### 生命周期的开始

- AgoraRtmLocalInvitation 由主叫通过显式调用 initWithCalleeId 方法创建，供主叫指定被叫、设置自定义内容，或查询呼叫邀请状态。
- AgoraRtmRemoteInvitation 在被叫收到呼叫邀请时由 SDK 创建并通过 remoteInvitationReceived 回调返回，供被叫设置相应内容、查询主叫 ID，或查询呼叫邀请状态。

### 生命周期的结束

AgoraRtmLocalInvitation 的生命周期在主叫收到以下回调时结束，并由 SDK 自动释放：

- [localInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationCanceled:)：主叫已成功取消呼叫邀请。
- [localInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationAccepted:withResponse:)：被叫已接受呼叫邀请。
- [localInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationRefused:withResponse:)：被叫已拒绝呼叫邀请。
- [localInvittionFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationFailure:errorCode:)：呼叫邀请过程已开始但是以失败告终。该回调通过错误码 [AgoraRtmLocalInvitationErrorCode](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationErrorCode.html) 概括了失败的主要原因：
  - `AgoraRtmLocalInvitationErrorRemoteOffline`：被叫不在线。
  - `AgoraRtmLocalInvitationErrorRemoteNoResponse`：被叫无响应（呼叫邀请发出 30 秒后仍未收到被叫 ACK 响应）
  - `AgoraRtmLocalInvitationErrorExpire`：呼叫邀请过期（被叫收到呼叫邀请 60 秒后呼叫邀请未被取消、接受，或拒绝）

AgoraRtmRemoteInvitation 的生命周期在被叫收到以下回调时结束，并由 SDK 自动释放：

- [remoteInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationCanceled:)：主叫已成功取消呼叫邀请。
- [remoteInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationAccepted:)：接受呼叫邀请成功。
- [remoteInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationRefused:)：拒绝呼叫邀请成功。
- [remoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationFailure:errorCode:)：呼叫邀请过程以失败告终。该回调通过错误码 [AgoraRtmRemoteInvitationErrorCode](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationErrorCode.html) 概括了失败的主要原因：
  - `AgoraRtmRemoteInvitationErrorLocalOffline`：接受呼叫邀请时主叫不在线。
  - `AgoraRtmRemoteInvitationErrorAcceptFailure`：被叫接受呼叫邀请后，主叫未响应。
  - `AgoraRtmRemoteInvitationErrorExpire`：呼叫邀请过期（被叫收到呼叫邀请 60 秒后呼叫邀请未被取消、接受，或拒绝）

> 被叫接受来自主叫的呼叫邀请是一个三次握手的过程：
>
> - 一次握手：主叫调用 sendLocalInvitation 发送呼叫邀请，被叫收到 onRemoteInvitationReceived 回调；
> - 二次握手：被叫调用 [acceptRemoteinvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/acceptRemoteInvitation:completion:) 方法接受呼叫邀请，主叫收到被叫已接受呼叫邀请的信息；
> - 三次握手：被叫确认主叫已经收到了被叫接受邀请的信息，呼叫邀请成功。

## 通用行为限定 

了解了呼叫邀请生命周期定义后，我们就能够理解 Agora RTM SDK 对于呼叫邀请的行为限定：所有的呼叫邀请操作都应在呼叫邀请的生命周期内进行。

在主叫显式调用 [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:) 或 [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:) 方法发送或取消呼叫邀请时，或当被叫显式调用 [acceptRemoteinvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/acceptRemoteInvitation:completion:) 或 [refuseRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/refuseRemoteInvitation:completion:) 方法接受或拒绝呼叫邀请时，SDK 会对 APP 行为进行检查并通过 [localInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationFailure:errorCode:) 或 [remoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationFailure:errorCode:) 回调返回相应的 [AgoraRtmInvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationErrorCode.html) 错误码：

| 回调                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `AgoraRtmInvitationApiCallErrorNotStarted` | 呼叫邀请未开始。错误情况包括：在 sendLocalInvitation 之前调用了 cancelLocalInvitation。 |
| `AgoraRtmInvitationApiCallErrorAlreadyEnd` | 呼叫邀请结束后又调用了 send、cancel、accept，或 refuse。     |
| `AgoraRtmInvitationApiCallErrorAlreadyAccept` | 被叫重复调用 accept 方法。                                   |
| `AgoraRtmInvitationApiCallErrorAlreadySent` | 主叫重复调用 send 方法发送呼叫邀请。                         |

## 状态流转图

呼叫邀请中，主叫可以通过 [AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 对象的 [state](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html#//api/name/state) 属性查询当前呼叫邀请的有关状态；被叫可以通过 SDK 返回的 [AgoraRtmRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) 对象的 [state](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html#//api/name/state) 属性查询当前呼叫邀请的相关状态。

### LocalInvitationState 

下图描述了与主叫相关的呼叫邀请状态流转图：

![](https://web-cdn.agora.io/docs-files/1582270646018)

### RemoteInvitationState 

下图描述了与被叫相关的呼叫邀请状态流转图：

![](https://web-cdn.agora.io/docs-files/1582270656158)

## API 时序图

### 取消已发送呼叫邀请

![](https://web-cdn.agora.io/docs-files/1565677879249)

### 接受／拒绝呼叫邀请

![](https://web-cdn.agora.io/docs-files/1565670138257)

## 注意事项及限制条件

- 主叫设置的呼叫邀请 content 的字符串长度：8 KB，格式为 UTF-8。
- 被叫设置的呼叫邀请响应 response 的字符串长度：8 KB，格式为 UTF-8。
- 呼叫邀请的 channel ID 仅用于与老信令互通时设置。设置的 channel ID 必须与老信令 SDK 设置相同才能实现互通。字符串长度：64 字节，格式为 UTF-8。



## API 参考

详见：[呼叫邀请管理](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/docs/API-Overview.html#callinvitation)。

## 示例项目

我们在 GitHub 提供一个开源的[示例项目](https://github.com/AgoraIO-Usecase/Video-Calling)，你也可以前往下载体验并参考源代码。
