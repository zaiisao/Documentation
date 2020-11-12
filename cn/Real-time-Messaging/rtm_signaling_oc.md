
---
title: 信令 与 RTM 功能对照表
description: 
platform: iOS,macOS
updatedAt: Tue Nov 10 2020 05:30:52 GMT+0800 (CST)
---
# 信令 与 RTM 功能对照表
本页对比 Agora Signaling SDK 与 Agora RTM SDK 的区别。

## 登录登出

| 方法         | 信令                                          | RTM 实时消息                       |
| ------------ | --------------------------------------------- | ---------------------------------- |
| 创建实例     | `getInstanceWithoutMedia`/`createSigInstance` | [initWithAppId](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:)<sup>1</sup>        |
| 登录         | `login`/`login2`                              | [loginByToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:)<sup>2</sup>                |
| 登出         | `logout`                                      | [logoutWithCompletion](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/logoutWithCompletion:)                           |
| 获取登录状态 | `getStatus`                                   | N/A。详见 [connectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:connectionStateChanged:reason:) |
| 销毁实例     | `destroy`                                     | N/A                                |

| 事件         | 信令                  | RTM 实时消息             |
| ------------ | --------------------- | ------------------------ |
| 登录成功     | `onLoginSuccess`      | [AgoraRtmLoginBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLoginBlock.html)     |
| 登录失败     | `onLoginFailed`       | [AgoraRtmLoginBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLoginBlock.html)     |
| 登出结果     | `onLogout`            | [AgoraRtmLogoutBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLogoutBlock.html)    |
| 连接状态改变 | N/A。详见 `getStatus` | [connectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:connectionStateChanged:reason:) |

> - 若无特别说明，Agora RTM Objective-C SDK for iOS/macOS 的所有核心 API 都应在调用 [loginByToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) 方法成功并收到 `AgoraRtmLoginErrorOk` 错误码后调用。Agora Signaling SDK 只允许你每次进入一个频道。
> - <sup>1</sup> 你可以通过调用 [initWithAppId](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:) 方法创建多个 AgoraRtmKit 实例。Agora RTM SDK 不会限制你创建 AgoraRtmKit 实例的个数，但某个 AgoraRtmKit 实例最多只能同时加入 20 个 AgoraRtmChannel 频道。
> - <sup>2</sup> RTM 的 Token 生成方式与老信令的 Token 生成方式完全不同。详见[校验用户权限](../../cn/Real-time-Messaging/rtm_token.md)。
> - <sup>2</sup> 信令 Token 采用的 "\_no\_need\_token" 机制不适用于 RTM Token。 
> - <sup>2</sup> Agora RTM SDK 连接或重连 Agora RTM 系统的方式也完全不同。详情请见[连接状态管理](../../cn/Real-time-Messaging/reconnecting_oc.md)。 

## 发送点对点消息

| 方法           | 信令                 | RTM 实时消息                                        |
| -------------- | -------------------- | --------------------------------------------------- |
| 创建消息实例   | N/A                  | [initWithText](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html#//api/name/initWithText:)<sup>1</sup>                          |
| 发送点对点消息 | `messageInstantSend` | [sendMessage:toPeer:sendMessageOptions:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:) |

| 事件               | 信令                      | RTM 实时消息                               |
| ------------------ | ------------------------- | ------------------------------------------ |
| 点对点消息发送成功 | `onMessageSendSuccess`    | [AgoraRtmSendPeerMessageBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendPeerMessageBlock.html)<sup>2</sup> |
| 点对点消息发送失败 | `onMessageSendError`      | [AgoraRtmSendPeerMessageBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendPeerMessageBlock.html)             |
| 收到一条点对点消息 | `onMessageInstantReceive` | [rtmKit:messageReceived:fromPeer:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:messageReceived:fromPeer:)         |

> <sup>1</sup> 使用 Agora RTM SDK 发送消息之前你必须创建一个消息实例。消息实例既适用于点对点消息也适用于频道消息。Agora RTM SDK 自版本 v0.9.3 起支持通过设置 [sendMessageOptions](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmSendMessageOptions.html) 发送点对点的离线消息。
>
> 2 Agora Signaling SDK 会在服务端收到点对点消息时返回 `onMessageSendSuccess` 回调而 Agora RTM SDK 会在指定用户收到点对点消息后返回 `AgoraRtmSendPeerMessageErrorOk` 错误码。

## 查询指定用户的在线状态

| 方法                   | 信令              | RTM 实时消息             |
| ---------------------- | ----------------- | ------------------------ |
| 查询指定用户的在线状态 | `queryuserStatus` | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) |
|                        |                   |                          |



| 事件         | 信令                      | RTM 实时消息                    |
| ------------ | ------------------------- | ------------------------------- |
| 返回查询结果 | `OnQueryUserStatusResult` | [AgoraRtmQueryPeersOnlineBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmQueryPeersOnlineBlock.html) |

> Agora RTM SDK 允许你查询一组用户的在线状态，而不只一个用户的在线状态。

## 用户属性相关操作

| 方法                       | 信令             | RTM 实时消息                          |
| -------------------------- | ---------------- | ------------------------------------- |
| 设置本地用户属性           | `setAttr`        | [addOrUpdateLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateLocalUserAttributes:completion:)      |
| 获取本地用户的某个属性     | `getAttr`        | [getUserAttributesBykeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:)<sup>1</sup> |
| 获取本地用户的全部属性     | `getAttrAll`     | [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)<sup>2</sup>       |
| 获取指定用户的某个属性     | `getUserAttrAll` | [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)                   |
| 全量替换本地用户的全部属性 | N/A              | [setLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLocalUserAttributes:completion:)              |
| 删除本地用户的指定属性     | N/A              | [deleteLocaluserAttributeByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteLocalUserAttributesByKeys:completion:)      |
| 清空本地用户全部属性       | N/A              | [clearLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearLocalUserAttributesWithCompletion:)            |
|                            |                  |                                       |

| 事件                   | 信令                    | RTM 实时消息                                                 |
| ---------------------- | ----------------------- | ------------------------------------------------------------ |
| 返回用户属性相关的结果 | `onUserAttributeResult` | <li>[AgoraRtmSetLocalUserAttributesBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSetLocalUserAttributesBlock.html) <li>[AgoraRtmAddOrUpdateLocalUserAttributesBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmAddOrUpdateLocalUserAttributesBlock.html) <li>[AgoraRtmDeleteLocalUserAttributesBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmDeleteLocalUserAttributesBlock.html) <li> [AgoraRtmClearLocalUserAttributesBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearLocalUserAttributesWithCompletion:) <li> [AgoraRtmGetUserAttributesBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmGetUserAttributesBlock.html) |

> - <sup>1</sup> [getUserAttributesBykeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:) 方法允许你获取本地用户的一系列属性。
> - <sup>2</sup> [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:) 方法允许你获取本地用户或指定远端用户的用户属性。

## 加入离开频道相关

| 方法         | 信令           | RTM 实时消息                      |
| ------------ | -------------- | --------------------------------- |
| 创建频道实例 | N/A            | [createChannelWithId](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:)<sup>1</sup> |
| 加入指定频道 | `channelJoin`  | [joinWithCompletion](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:)<sup>2</sup>  |
| 离开频道     | `channelLeave` | [leaveWithCompletion](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/leaveWithCompletion:)             |

| 事件                       | 信令                  | RTM 实时消息                |
| -------------------------- | --------------------- | --------------------------- |
| 成功加入指定频道           | `onChannelJoined`     | [AgoraRtmJoinChannelBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmJoinChannelBlock.html)  |
| 加入指定频道失败           | `onChannelJoinFailed` | [AgoraRtmJoinChannelBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmJoinChannelBlock.html)  |
| 远端用户加入当前频道       | `onChannelUserJoined` | [memberJoined](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:memberJoined:)              |
| 成功离开当前频道           | `onChannelLeaved`     | [AgoraRtmLeaveChannelBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLeaveChannelBlock.html) |
| 远端频道成员离开当前频道   | `onChannelUserLeaved` | [memberLeft](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:memberLeft:)                |
| 加入频道时返回频道成员列表 | `onChannelUserList`   | N/A<sup>3</sup>             |
|                            |                       |                             |

> - <sup>1</sup> Agora RTM SDK 要求你在加入频道前先创建频道实例。
> - <sup>2</sup> Agora RTM SDK 允许你最多同时加入 20 个频道。 
> - <sup>3</sup> Agora RTM SDK 不会在用户成功加入频道时返回当前频道成员列表。
> - Agora RTM SDK 不支持特殊频道功能。

## 频道消息

| 方法                 | 信令                      | RTM 实时消息                          |
| -------------------- | ------------------------- | ------------------------------------- |
| 创建消息实例         | N/A                       | [initWithText](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html#//api/name/initWithText:)<sup>1</sup>            |
| 从频道内发送频道消息 | `messageChannelSend`      | [sendMessage:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:)<sup>2</sup> |
| 从频道外发送频道消息 | `messageChannelSendForce` | N/A                                   |



| 事件             | 信令                      | RTM 实时消息                      |
| ---------------- | ------------------------- | --------------------------------- |
| 频道消息发送成功 | `onMessageSendSuccess`    | [AgoraRtmSendChannelMessageBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendChannelMessageBlock.html) |
| 频道消息         | `onMessageSendError`      | [AgoraRtmSendChannelMessageBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendChannelMessageBlock.html) |
| 收到一条频道消息 | `onMessageChannelReceive` | [messageReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:messageReceived:fromMember:)<sup>3</sup>     |



> - <sup>1</sup> Agora RTM SDK 要求你在发送频道消息或点对点消息之前必须创建一个消息实例。
> - <sup>2</sup> Agora RTM SDK 暂不支持从频道外向频道内发送消息。你必须加入频道才能向该频道成员发送频道消息。不过，你也可以通过设置频道属性并在每次 API 调用时开启 enableNotificationToChannelMembers 通知该频道所有成员本次更新。
> - <sup>3</sup> 与 Agora Signaling SDK 的行为不同，[messageReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:messageReceived:fromMember:) 回调返回给频道内的其他频道成员，而非返回给发送人。

## 频道属性相关操作

| 方法                 | 信令               | RTM 实时消息 |
| -------------------- | ------------------ | ------------ |
| 设置一条频道属性     | `channelSetAttr`   | [addOrUpdateChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateChannel:Attributes:Options:completion:)<sup>1</sup>          |
| 删除一条频道属性     | `channelDelAttr`   | [deleteChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteChannel:AttributesByKeys:Options:completion:)<sup>1</sup>          |
| 删除频道的全部属性 | `channelClearAttr` | [clearChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearChannel:Options:AttributesWithCompletion:)<sup>1</sup>           |
| 全量设置某指定频道的属性 | N/A | [setChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setChannel:Attributes:Options:completion:)<sup>1</sup>   |
| 获取某指定频道的全部属性 | N/A | [getChannelAllAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAllAttributes:completion:) |
| 获取某指定频道指定属性名的属性 | N/A | [getChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAttributes:ByKeys:completion:) |

> <sup>1</sup> Agora RTM SDK 支持在不加入频道的情况下更新该频道的属性。

| 事件           | 信令                   | RTM 实时消息 |
| -------------- | ---------------------- | ------------ |
| 频道属性已更新 | `onChannelAttrUpdated` | [attributesUpdate](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:attributeUpdate:) <sup>2</sup>         |

> <sup>2</sup>  该回调不会默认触发。只有当频道属性更新者将 [enableNotificationToChannelMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannelAttributeOptions.html#//api/name/enableNotificationToChannelMembers) 设为 YES 后，该回调才会被触发。



## 获取当前频道用户列表

| 方法                   | 信令     | RTM 实时消息             |
| ---------------------- | -------- | ------------------------ |
| 获取指定频道的成员列表 | `invoke` | [getMembersWithCompletion](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:)<sup>1</sup> |



| 事件                   | 信令          | RTM 实时消息              |
| ---------------------- | ------------- | ------------------------- |
| 返回指定频道的成员列表 | `onInvokeRet` | [AgoraRtmGetMembersBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmGetMembersBlock.html) |

> <sup>1</sup> 你必须加入一个 RtmChannel 频道后才能获取该频道的成员列表。当频道人员超过 512 人上限，Agora RTM SDK 会随机返回频道内的 512 个当前频道成员。 



## 获取指定频道用户人数

| 方法                 | 信令                  | RTM 实时消息 |
| -------------------- | --------------------- | ------------ |
| 获取指定频道成员人数 | `channelQueryUserNum` | [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelMemberCount:completion:)<sup>1</sup>          |

> <sup>1</sup>  Agora RTM SDK 支持查询单个或多个频道的成员人数。

| 事件                   | 信令                          | RTM 实时消息 |
| ---------------------- | ----------------------------- | ------------ |
| 返回指定频道的用户人数 | `onChannelQueryUserNumResult` | [AgoraRtmChannelMemberCountBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmChannelMemberCountBlock.html)          |

## 呼叫邀请



| 方法                                          | 信令                                     | RTM 实时消息                        |
| --------------------------------------------- | ---------------------------------------- | ----------------------------------- |
| 创建 RTM 呼叫管理器                           | N/A                                      | [getRtmCallKit](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getRtmCallKit)<sup>1</sup>         |
| 供主叫创建并管理一个 `LocalInvitation` 实例。 | N/A                                      | [initWithCalleeId](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html#//api/name/initWithCalleeId:)<sup>2</sup>      |
| 供主叫向指定用户（被叫）发送呼叫邀请          | `channelInviteUser`/`channelInviteUser2` | [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:)<sup>3</sup>   |
| 供主叫发送 DTMF 呼叫邀请。                    | `channelInviteDTMF`                      | N/A                                 |
| 供主叫取消一个发出的呼叫邀请                  | `channelInviteEnd`                       | [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:)<sup>4</sup> |
| 供被叫接收一个呼叫邀请                        | `channelInviteAccept`                    | [acceptRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/acceptRemoteInvitation:completion:)            |
| 供被叫拒绝一个呼叫邀请                        | `channelInviteRefuse`                    | [refuseRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/refuseRemoteInvitation:completion:)            |


> - <sup>1</sup> Agora RTM SDK 要求主叫或被叫在发送、取消、接收或拒绝一个呼叫邀请前必须创建一个 [AgoraRtmCallKit](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html) 实例。
> - <sup>2</sup> Agora RTM SDK 引入了 [AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 和 [AgoraRtmRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) 对象。前者由主叫通过调用 [initWithCalleeId](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html#//api/name/initWithCalleeId:) 方法创建，后者在被叫收到呼叫邀请时由 SDK 自动创建。你可以将这两个对象理解为同一个呼叫邀请的两种不同形式。主叫通过 [AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 对象指定被叫，设置自定义内容或检查 [AgoraRtmLocalInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationState.html) 状态，被叫通过 [AgoraRtmRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) 对象设置响应内容，检查主叫 ID，或者检查 [AgoraRtmRemoteInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationState.html) 状态。
> - <sup>3</sup> [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:) 函数不带 `channelInviteUser2` 函数中的 `extra` 参数。 
> - <sup>4</sup> `channelInviteEnd` 方法可以在任意时间取消一个呼叫邀请，而 [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:) 方法只能取消一个已发送的正在进行的呼叫邀请。
> - 为了和 Agora Signaling SDK 互通，你必须把你的 Agora RTM SDK 版本升级到 v1.0 以上并设置 channel ID。请注意即使被叫接收了呼叫邀请，Agora RTM SDK 也不会将主叫或被叫加入指定频道。

| 同步回调     | 信令 | RTM 实时消息                                                 |
| ------------ | ---- | ------------------------------------------------------------ |
| 方法调用成功 | N/A  | <li> [AgoraRtmLocalinvitationSendBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:) <li> [AgoraRtmLocalInvitationCancelBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLocalInvitationCancelBlock.html) <li> [AgoraRtmRemoteInvitationAcceptBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationAcceptBlock.html) <li> [AgoraRtmRemoteinvitationRefuseBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationRefuseBlock.html)                                                  |
| 方法调用失败 | N/A  | <li> [AgoraRtmLocalinvitationSendBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:) <li> [AgoraRtmLocalInvitationCancelBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLocalInvitationCancelBlock.html) <li> [AgoraRtmRemoteInvitationAcceptBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationAcceptBlock.html) <li> [AgoraRtmRemoteinvitationRefuseBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationRefuseBlock.html) 。错误码详见 [AgoraRtmInvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmInvitationApiCallErrorCode.html)<sup>5</sup> |

> <sup>5</sup> 如果用户在 [AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 生命周期开始之前或生命周期结束之后调用了 [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:)、 [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:)、 [acceptRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/acceptRemoteInvitation:completion:) 或 [refuseRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/refuseRemoteInvitation:completion:) ，Agora RTM SDK 会返回 [AgoraRtmLocalInvitationSendBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:)、[AgoraRtmLocalInvitationCancelBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLocalInvitationCancelBlock.html)、[AgoraRtmRemoteInvitationAcceptBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationAcceptBlock.html) 或 [AgoraRtmRemoteInvitationRefuseBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationRefuseBlock.html) 回调以及 [AgoraRtmInvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmInvitationApiCallErrorCode.html) 错误码。



| 事件                           | 信令                     | RTM 实时消息                                                 |
| ------------------------------ | ------------------------ | ------------------------------------------------------------ |
| 返回给主叫：被叫已收到呼叫邀请 | `oninviteReceivedByPeer` | [localInvitationReceivedByPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationReceivedByPeer:)                              |
| 返回给主叫：呼叫邀请已被取消   | `onInviteEndByMyself`    | [localInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationCanceled:)                                    |
| 返回给主叫：被叫已接收呼叫邀请 | `onInviteAcceptedByPeer` | [localInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationAccepted:withResponse:)                                    |
| 返回给主叫：被叫已拒绝呼叫邀请 | `onInviteRefusedByPeer`  | [localInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationRefused:withResponse:)                                     |
| 返回给主叫：呼叫邀请过程失败   | `onInviteFailed`         | [localInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationFailure:errorCode:)。错误码详见 [AgoraRtmLocalInvitationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationErrorCode.html)。<sup>6</sup> |
|                                |                          |                                                              |

> <sup>6</sup>: 如果呼叫邀请过程已经开始但以失败告终，Agora RTM SDK 会返回 [localInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationFailure:errorCode:) 。场景包括：被叫离线，[AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 对象发送超时 [AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 过期，或者被叫收到了呼叫邀请但未能在指定时间内响应呼叫邀请。

| 事件                               | 信令                | RTM 实时消息                                                 |
| ---------------------------------- | ------------------- | ------------------------------------------------------------ |
| 返回给 DTMF 用户：收到一个呼叫邀请 | `onInviteMsg`       | N/A                                                          |
| 返回给被叫：收到一个呼叫邀请       | `oninviteReceived`  | [remoteInvitationReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationReceived:)                                   |
| 返回给被叫：主叫已取消呼叫邀请     | `onInviteEndByPeer` | [remoteInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationCanceled:)                                   |
| 返回给被叫：已成功接受呼叫邀请     | N/A                 | [remoteInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationAccepted:)                                   |
| 返回给被叫：已拒绝呼叫邀请         | N/A                 | [remoteInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationRefused:)                                    |
| 返回给被叫：呼叫邀请过程失败       | N/A                 | [remoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationFailure:errorCode:)。错误码详见 [AgoraRtmRemoteInvitationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationErrorCode.html)。<sup>7</sup> |
|                                    |                     |                                                              |

> <sup>7</sup> 如果呼叫邀请进程已经开始但以失败告终，Agora RTM SDK 会返回 [remoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationFailure:errorCode:) 回调给被叫。通用场景包括：[AgoraRtmRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) 发送超时或 [AgoraRtmRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) 过期。 

## 更新 Token



| 方法           | 信令 | RTM 实时消息 |
| -------------- | ---- | ------------ |
| 更新当前 Token | N/A  | [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) |



| 事件             | 信令 | RTM 实时消息            |
| ---------------- | ---- | ----------------------- |
| 返回方法调用结果 | N/A  | [AgoraRtmRenewTokenBlock](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRenewTokenBlock.html) |
| Token 已过期     | N/A  | [rtmKitTokenDidExpire](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKitTokenDidExpire:)  |


