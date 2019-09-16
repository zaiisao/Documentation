
---
title: 发版说明
description: 
platform: iOS,macOS
updatedAt: Mon Sep 16 2019 13:57:21 GMT+0800 (CST)
---
# 发版说明
## 简介

Agora RTM SDK 提供了稳定可靠、低延时、高并发的全球消息云服务，帮助你快速构建实时通信场景,  可实现消息通道、呼叫、聊天、状态同步等功能。点击 [实时消息产品概述](../../cn/Real-time-Messaging/RTM_product.md) 了解更多详情。

## 1.0.1 版

该版本于 2019 年 8 月 1 日发布。

### 问题修复

- 断网后 SDK 未返回 `connectionStateChanged` 回调。
- iOS 平台偶现的崩溃。

## 1.0.0 版

该版本于 2019 年 7 月 24 日发布。

### 新增功能

#### 新老互通

支持与 Agora Signaling SDK 互通。

本版本在 `AgoraRtmLocalInvitation` 类中实现了 `channelId` 属性。

> - 如需与 Agora Signaling SDK 互通，则必须设置 `channelId` 。不过即使被叫成功接受呼叫邀请，Agora RTM SDK 也不会将主叫或被叫加入指定频道。
> - 如果你的应用不涉及 Agora Signaling SDK，我们推荐使用 `AgoraRtmLocalInvitation` 类中的 `content` 属性或者`AgoraRtmRemoteInvitation` 类中的 `response` 属性设置自定义内容。

#### 设置日志文件地址

支持通过调用 `setLogFile` 方法变更本地日志的默认地址。该方法无需在 `loginByToken` 成功之后调用，我们建议在创建并初始化 `AgoraRtmkit` 实例后即调用该方法，否则会造成日志文件显示不完整。

#### 设置日志输出等级

支持通过调用 `setLogFilter` 方法将日志内容按照 OFF、CRITICAL、ERROR、WARNING 和 INFO 不同等级输出。详见 [AgoraRtmLogFilter](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLogFilter.html) 。

> 该方法无需在 `loginByToken` 成功之后调用。

#### 设置日志文件大小

支持通过 `setLogFileSize` 方法设置日志文件大小。日志的默认大小为 512 KB。低于该默认大小的设置无效。

> 该方法无需在 `loginByToken` 成功之后调用。

#### 查询 SDK 版本信息

支持通过调用静态方法 `getSDKVersion` 查询 Agora RTM SDK 的版本信息。

###  功能改进

针对以下不同错误情况细化了错误代码

- Agora RTM 服务未初始化
- 调用频率超过上限
- 未调用 `loginByToken` 方法或 `loginByToken` 方法未调用成功

### 问题修复

- 修复 iOS 平台偶现的崩溃。
- 修复了一个可以用静态 App ID 和一个通过动态 App ID 生成的 Token 登录Agora RTM 系统的问题。


### API 变更

- [setLogFile](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLogFile:)：设定日志文件的默认地址。
- [setLogFilter](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLogFilters:)：设置日志输出等级。
- [setLogFileSize](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLogFileSize:)：设置日志文件大小。
- [getSDKVersion](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getSDKVersion)：获取 Agora RTM SDK 的版本信息。



## 0.9.3 版

该版本于 2019 年 6 月 7 日发布。

### 新增功能

#### 发送（离线）点对点消息

本版本支持发送离线消息。在开通离线消息后，用户不必等到接收端上线才能发送点对点消息。如果对端离线，消息服务器会为每个接收端存储 200 条离线消息长达七天。消息以队列形式存储。当离线消息超限时，最新存储的消息会导致最老的消息丢失。当发送端设置了离线消息，而此时消息接收端不在线，发送端会收到错误码：[AgoraRtmSendPeerMessageErrorCachedByServer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmProcessAttributeErrorCode.html)

> 该方法的调用频率限制为每秒 60 条（点对点消息和频道消息一并计算在内）。

#### 设置本地用户属性、查询指定用户属性

本版本支持设置和查询用户属性。每个用户属性为 key 和 value 的键值对。每个属性的 key 为 32 字节可见字符，每个属性的 value 的字符串长度不得超过 8 KB。单个用户的全部属性长度不得超过 16 KB。以下为本版本支持内容：

   - 全量设置本地用户属性
   - 增加或更新本地用户属性
   - 删除本地用户指定属性
   - 清空本地用户属性
   - 全量获取指定用户属性
   - 获取指定用户指定属性。


> - 用户属性的相关操作必须在登录 Agora RTM 系统成功后才能进行，否则 SDK 会返回错误码：`AgoraRtmAttributeOperationErrorNotReady`
> - 设置的用户属性会在用户登出 Agora RTM 系统后自动失效。
> - 单次用户属性设置操作不得超过 16 KB，否则 SDK 会返回错误码：`AgoraRtmAttributeOperationErrorSizeOverflow` 。
> - 用户属性设置相关操作的调用频率限制为每 5 秒 10 条，超限则 SDK 会返回错误码：`AgoraRtmAttributeOperationErrorTooOften`。
> - 获取用户属性相关操作的调用频率为每 5 秒 40 条，超限则 SDK 会返回错误码：`AgoraRtmAttributeOperationErrorTooOften` 。

### API 变更

#### 新增：

- [sendMessageToPeer()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:):  发送（离线）点对点消息给指定用户。
- [serverReceivedTs](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html#//api/name/serverReceivedTs)： 消息接收者查询服务器的接收消息时间。
- [isOfflineMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html#//api/name/isOfflineMessage)：消息接收者查询消息是否为离线消息。
- [AgoraRtmSendPeerMessageErrorCachedByServer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmProcessAttributeErrorCode.html)：对端用户不在线，离线消息会被消息服务器存储。
- [setLocalUserAttributes:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLocalUserAttributes:completion:)：全量设置本地用户属性
- [addOrUpdateLocalUserAttributes:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateLocalUserAttributes:completion:)：增加本地用户属性或更新本地用户属性
- [deleteLocalUserAttributesByKeys:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteLocalUserAttributesByKeys:completion:)：删除本地用户指定属性
- [clearLocalUserAttributesWithCompletion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearLocalUserAttributesWithCompletion:)：清空本地用户全部属性
- [getUserAllAttributes:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)：获取指定用户的全部属性
- [getUserAttributes:ByKeys:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:): 获取指定用户的指定属性
- [	AgoraRtmProcessAttributeErrorCode](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmProcessAttributeErrorCode.html)：属性操作相关错误码

### 性能改进

- 支持在登录 Agora RTM 系统之前创建频道实例。
- 取消创建 RTM 频道最多 20 个的限制，但是同一用户只能同时加入 20 个频道，超限后会收到错误码 `AgoraRtmJoinChannelErrorFailure ` 。


### 问题修复

- 偶现的系统崩溃。
- 用户登出后，其它用户查询该用户仍然显示在线，30 秒后查询不在线。


## 0.9.2 版

该版本于 2019 年 5 月 8 日发布。

> 当前版本不支持在 [登录](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) RTM 系统前 [创建频道实例](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) 。

### 新增功能

#### 查询用户在线状态

本版本引入了新的概念：在线和离线。一般情况下：

- 在线：用户已登录到 Agora RTM 系统。
- 离线：用户已登出 Agora RTM 系统或因其他原因，比如权限或网络原因，与 Agora RTM 系统断开连接。

本版本增加了查询用户在线状态功能。你可以在登录 Agora RTM 系统后查询最多 256 个指定用户的在线状态。详见： [queryPeersOnlineStatus:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) 接口。

> - 返回的阵列中的 Use ID 的顺序以输入顺序为准。
> - 该方法的调用频率限制为每 5 秒 10 次。详见 [限制条件](../../cn/Real-time-Messaging/RTM_limitations_ios.md) 。


#### 更新 Token

在安全性要求较高的情况下，你需要输入 Token [登录](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) Agora RTM 系统。 每个 Token 都会在生成 24 小时后过期，本版本提供了 [更新 Token](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) 的功能。

- 在登录 Agora RTM 系统时，如果你的 Token 已过期，SDK 会返回 [AgoraRtmLoginErrorTokenExpired](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLoginErrorCode.html) 错误码提示 Token 过期。
- 在已登录 Agora RTM 系统的情况下，Token 过期不会导致用户立即被踢出，但当用户重新登录 Agora RTM 系统时需要调用。所以，我们建议你在收到 [rtmKitTokenDidExpire:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKitTokenDidExpire:) 回调后尽快更新 Token。


> - [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353) 方法必须在 [创建 RtmClient 实例](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:) 后才能调用。
> - Agora RTM Token 的生成方式、输入参数与 Agora 媒体 SDK 不同，详情请见： [校验用户权限](../../cn/Real-time-Messaging/RTM_key.md) 。
> - `renewToken:completion:` 方法的调用频率为 2 次 / 秒。详见 [限制条件](../../cn/Real-time-Messaging/RTM_limitations_ios.md) 。




### API 变更

#### 查询用户在线状态相关

##### 新增

- 方法： [queryPeersOnlineStatus:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:)
- 错误码：[AgoraRtmQueryPeersOnlineErrorCode](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmQueryPeersOnlineErrorCode.html)

#### 更新 Token 相关

##### 新增

- 方法： [renewToken:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:)
- 回调： [rtmKitTokenDidExpire:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKitTokenDidExpire:)
- 错误码： [AgoraRtmRenewTokenErrorCode](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRenewTokenErrorCode.html)

#### 呼叫邀请相关

新增以下错误码覆盖用户在没有登录 Agora RTM 系统的情况下发送呼叫邀请的错误情况：

- [AgoraRtmLocalInvitationErrorNotLoggedIn](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationErrorCode.html)




## 0.9.1 版

该版本于 2019 年 4 月 4 日发布。

> 本版本不包含 `setLogFile` 和 `setLogFilter` 方法。所有的日志信息默认保存在：
> - iOS 平台： **/Library/Caches/agorartm.log** 。
> -macOS 平台： **~/Library/Logs/agorartm.log** 。

### 新增功能

本版本增加了呼叫邀请功能。结合音视频一对一或一对多通话场景，你可以创建、发送、取消、接受或拒绝一个呼叫邀请。



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


