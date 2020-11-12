
---
title: 信令 与 RTM 功能对照表
description: 
platform: Web
updatedAt: Tue Nov 10 2020 05:31:05 GMT+0800 (CST)
---
# 信令 与 RTM 功能对照表
本页对比 Agora Signaling SDK 与 Agora RTM SDK 的区别。

## 登录登出

| 方法         | 信令                                   | RTM 实时消息                                                 |
| ------------ | -------------------------------------- | ------------------------------------------------------------ |
| 创建实例     | `Signal` | [createInstance](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/modules/agorartm.html#createinstance)<sup>1</sup> |
| 登录         | `login`/`login2`                       | [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login)<sup>2</sup> |
| 登出         | `logout`                               | [logout](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#logout) |
| 获取登录状态 | `getStatus`                            | N/A。详见 [onConnectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#connectionstatechanged) |

| 事件         | 信令                  | RTM 实时消息                                                 |
| ------------ | --------------------- | ------------------------------------------------------------ |
| 登录成功     | `onLoginSuccess`      | Promise 会在用户成功登录 Agora RTM 系统后 resolve。 |
| 登录失败     | `onLoginFailed`       | 请使用 Promise 的 `catch` 方法获取并处理异常。 |
| 登出结果     | `onLogout`            | Promise 会在用户成功登出 Agora RTM 系统后 resolve。 |
| 连接状态改变 | N/A。详见 `getStatus` | [onConnectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#connectionstatechanged) |

> - 若无特别说明，Agora RTM Android SDK 的所有核心 API 都应在调用 [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login) 方法成功登录 Agora RTM 系统后调用。Agora Signaling SDK 只允许你每次进入一个频道。
> - <sup>1</sup> 你可以通过调用 [createInstance](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/modules/agorartm.html#createinstance) 方法创建多个 RtmClient 实例。Agora RTM SDK 不会限制你创建 RtmClient 实例的个数，但某个 RtmClient 最多只能同时加入 20 个 RtmChannel 频道。
> - <sup>2</sup> RTM 的 Token 生成方式与老信令的 Token 生成方式完全不同。详见[校验用户权限](../../cn/Real-time-Messaging/rtm_token.md)。
> - <sup>2</sup> 信令 Token 采用的 "\_no\_need\_token" 机制不适用于 RTM Token。 
> - <sup>2</sup> Agora RTM SDK 连接或重连 Agora RTM 系统的方式也完全不同。详情请见[连接状态管理](../../cn/Real-time-Messaging/reconnecting_android.md)。 

## 发送点对点消息

| 方法           | 信令                 | RTM 实时消息                                                 |
| -------------- | -------------------- | ------------------------------------------------------------ |
| 创建消息实例   | N/A                  | N/A  |
| 发送点对点消息 | `messageInstantSend` | [sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#sendmessagetopeer)<sup>1</sup> |

| 事件               | 信令                      | RTM 实时消息                                                 |
| ------------------ | ------------------------- | ------------------------------------------------------------ |
| 点对点消息发送成功 | 详见 `messageInstantSend` 的 `cb` 参数   | Promise<sup>2</sup> |
| 点对点消息发送失败 | 详见 `messageInstantSend` 的 `cb` 参数      | Promise |
| 收到一条点对点消息 | `onMessageInstantReceive` | [MessageFromPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#messagefrompeer) |

> <sup>1</sup> Agora RTM SDK 自版本 v0.9.3 起支持通过设置 `sendMessageOptions` 发送点对点的离线消息。
>
> <sup>2</sup> Agora Signaling SDK 会在服务端收到点对点消息时返回 `onMessageSendSuccess` 回调而 Agora RTM SDK 会在指定用户收到点对点消息后Promise 返回 `resolve` 状态。

## 查询指定用户的在线状态

| 方法                   | 信令              | RTM 实时消息                                                 |
| ---------------------- | ----------------- | ------------------------------------------------------------ |
| 查询指定用户的在线状态 | `invoke` | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersonlinestatus) |




| 事件         | 信令                      | RTM 实时消息                                                 |
| ------------ | ------------------------- | ------------------------------------------------------------ |
| 返回查询结果 | `OnQueryUserStatusResult` | Promise |

> Agora RTM SDK 允许你查询一组用户的在线状态，而不只一个用户的在线状态。

## 用户属性相关操作

| 方法                       | 信令             | RTM 实时消息                          |
| -------------------------- | ---------------- | ------------------------------------- |
| 设置本地用户属性           | `invoke`        | [addOrUpdateLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatelocaluserattributes)     |
| 获取本地用户的某个属性     | `invoke`        | [getUserAttributesBykeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys)<sup>1</sup> |
| 获取本地用户的全部属性     | `invoke`     | [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)<sup>2</sup>       |
| 获取指定用户的某个属性     | `invoke` | [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)                   |
| 全量替换本地用户的全部属性 | N/A              | [setLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setlocaluserattributes)              |
| 删除本地用户的指定属性     | N/A              | [deleteLocaluserAttributeByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletelocaluserattributesbykeys)      |
| 清空本地用户全部属性       | N/A              | [clearLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearlocaluserattributes)            |
|                            |                  |                                       |

| 事件                   | 信令                    | RTM 实时消息            |
| ---------------------- | ----------------------- | ----------------------- |
| 返回用户属性相关的结果 | `onInvokeRet` | Promise |

> - <sup>1</sup> [getuserAttributesBykeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys) 方法允许你获取本地用户的一系列属性。
> - <sup>2</sup> [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes) 方法允许你获取本地用户或指定远端用户的用户属性。

## 加入离开频道相关

| 方法         | 信令           | RTM 实时消息                |
| ------------ | -------------- | --------------------------- |
| 创建频道实例 | N/A            | [createChannel](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#createchannel)<sup>1</sup> |
| 加入指定频道 | `channelJoin`  | [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join)<sup>2</sup>          |
| 离开频道     | `channelLeave` | [leave](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#leave)                    |

| 事件                       | 信令                  | RTM 实时消息     |
| -------------------------- | --------------------- | ---------------- |
| 成功加入指定频道           | `onChannelJoined`     | Promise     |
| 加入指定频道失败           | `onChannelJoinFailed` | Promise      |
| 远端用户加入当前频道       | `onChannelUserJoined` | [MemberJoined](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#memberjoined) |
| 成功离开当前频道           | `onChannelLeaved`     | Promise     |
| 远端频道成员离开当前频道   | `onChannelUserLeaved` | [MemberLeft](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#memberleft)   |
| 加入频道时返回频道成员列表 | `onChannelUserList`   | N/A<sup>3</sup>  |

> - <sup>1</sup> Agora RTM SDK 要求你在加入频道前先创建频道实例。
> - <sup>2</sup> Agora RTM SDK 允许你最多同时加入 20 个频道。 
> - <sup>3</sup> Agora RTM SDK 不会在用户成功加入频道时返回当前频道成员列表。
> - Agora RTM SDK 不支持特殊频道功能。

## 频道消息

| 方法                 | 信令                      | RTM 实时消息                |
| -------------------- | ------------------------- | --------------------------- |
| 创建消息实例         | N/A                       | N/A |
| 从频道内发送频道消息 | `messageChannelSend`      | [sendMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#sendmessage)<sup>1</sup>   |




| 事件             | 信令                      | RTM 实时消息                    |
| ---------------- | ------------------------- | ------------------------------- |
| 频道消息发送成功 | 详见 `messageChannelSend` 的 `cb` 参数    | Promise                     |
| 频道消息         | 详见 `messageChannelSend` 的 `cb` 参数      | Promise                     |
| 收到一条频道消息 | `onMessageChannelReceive` | [ChannelMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#channelmessage)<sup>2</sup> |



> - <sup>1</sup> Agora RTM SDK 暂不支持从频道外向频道内发送消息。你必须加入频道才能向该频道成员发送频道消息。不过，你也可以通过设置频道属性并在每次 API 调用时开启 enableNotificationToChannelMembers 通知该频道所有成员本次更新。
> - <sup>2</sup> 与 Agora Signaling SDK 的行为不同，[ChannelMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#channelmessage) 回调返回给频道内的其他频道成员，而非返回给发送人。

## 频道属性相关操作

| 方法                 | 信令               | RTM 实时消息 |
| -------------------- | ------------------ | ------------ |
| 设置一条频道属性     | `channelSetAttr`   | [addOrUpdateChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatechannelattributes)<sup>1</sup>          |
| 删除一条频道属性     | `channelDelAttr`   | [deleteChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletechannelattributesbykeys)<sup>1</sup>          |
| 删除频道的全部属性 | `channelClearAttr` | [clearChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearchannelattributes)<sup>1</sup>           |
| 全量设置某指定频道的属性 | N/A | [setChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setchannelattributes)<sup>1</sup>   |
| 获取某指定频道的全部属性 | N/A | [getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributes) |
| 获取某指定频道指定属性名的属性 | N/A | [getChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletechannelattributesbykeys) |

> <sup>1</sup> Agora RTM SDK 支持在不加入频道的情况下更新该频道的属性。

| 事件           | 信令                   | RTM 实时消息 |
| -------------- | ---------------------- | ------------ |
| 频道属性已更新 | 详见相应方法的 `cb` 参数 | [AttributesUpdated](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#attributesupdated)<sup>2</sup>         |

> <sup>2</sup>  该回调不会默认触发。只有当频道属性更新者将 [enableNotificationToChannelMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/channelattributeoptions.html#enablenotificationtochannelmembers) 设为 true 后，该回调才会被触发。

## 获取当前频道用户列表

| 方法                   | 信令     | RTM 实时消息             |
| ---------------------- | -------- | ------------------------ |
| 获取指定频道的成员列表 | `invoke` | [getMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#getmembers)<sup>1</sup> |



| 事件                   | 信令          | RTM 实时消息            |
| ---------------------- | ------------- | ----------------------- |
| 返回指定频道的成员列表 | `onInvokeRet` | Promise |

> <sup>1</sup> 你必须加入一个 RtmChannel 频道后才能获取该频道的成员列表。当频道人员超过 512 人上限，Agora RTM SDK 会随机返回频道内的 512 个当前频道成员。 



## 获取指定频道用户人数

| 方法                 | 信令                  | RTM 实时消息 |
| -------------------- | --------------------- | ------------ |
| 获取指定频道成员人数 | N/A | [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelmembercount)<sup>1</sup>          |

> <sup>1</sup>  Agora RTM SDK 支持查询单个或多个频道的成员人数。

| 事件                   | 信令                          | RTM 实时消息 |
| ---------------------- | ----------------------------- | ------------ |
| 返回指定频道的用户人数 | N/A | Promise          |



## 呼叫邀请



| 方法                                          | 信令                                     | RTM 实时消息                        |
| --------------------------------------------- | ---------------------------------------- | ----------------------------------- |

| 供主叫创建并管理一个 `LocalInvitation` 实例。 | N/A                                      | [createLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#createlocalinvitation)<sup>1</sup> |
| 供主叫向指定用户（被叫）发送呼叫邀请          | `channelInviteUser`/`channelInviteUser2` | [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send)<sup>2</sup>   |
| 供主叫发送 DTMF 呼叫邀请。                    | `channelInviteDTMF`                      | N/A                                 |
| 供主叫取消一个发出的呼叫邀请                  | `channelInviteEnd`                       | [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#cancel)<sup>3</sup> |
| 供被叫接收一个呼叫邀请                        | `channelInviteAccept`                    | [acceptRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept)            |
| 供被叫拒绝一个呼叫邀请                        | `channelInviteRefuse`                    | [refuseRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept)           |


> - <sup>1</sup> Agora RTM SDK 引入了 [LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) 和 [RemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html) 对象。前者由主叫通过调用 [createLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#createlocalinvitation) 方法创建，后者在被叫收到呼叫邀请时由 SDK 自动创建。你可以将这两个对象理解为同一个呼叫邀请的两种不同形式。主叫通过 [LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) 对象指定被叫，设置自定义内容或检查 [LocalInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmstatuscode.localinvitationstate.html) 状态，被叫通过 [RemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html) 对象设置响应内容，检查主叫 ID，或者检查 [RemoteInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmstatuscode.remoteinvitationstate.html) 状态。
> - <sup>2</sup> [send](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send) 函数不带 `channelInviteUser2` 函数中的 `extra` 参数。 
> - <sup>3</sup> `channelInviteEnd` 方法可以在任意时间取消一个呼叫邀请，而 [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#cancel) 方法只能取消一个已发送的正在进行的呼叫邀请。
> - 为了和 Agora Signaling SDK 互通，你必须把你的 Agora RTM SDK 版本升级到 v1.0 以上并设置 channel ID。请注意即使被叫接收了呼叫邀请，Agora RTM SDK 也不会将主叫或被叫加入指定频道。

| 同步回调     | 信令 | RTM 实时消息                                                 |
| ------------ | ---- | ------------------------------------------------------------ |
| 方法调用成功 | N/A  | <li>[LocalInvitationEvents](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html)<li> [RemoteInvitationEvents](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html)                                                 |
| 方法调用失败 | N/A  | <li>[LocalInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationfailure)<li> [RemoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html)。错误码详见 [InvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmerrorcode.invitationapicallerror.html)。<sup>5</sup> |

> <sup>5</sup> 如果用户在 [LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) 生命周期开始之前或生命周期结束之后调用了 [send](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send)、 [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#cancel)、 [accept](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept) 或 [refuse](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept)，Agora RTM SDK 会返回 [InvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmerrorcode.invitationapicallerror.html) 错误码。



| 事件                           | 信令                     | RTM 实时消息                                                 |
| ------------------------------ | ------------------------ | ------------------------------------------------------------ |
| 返回给主叫：被叫已收到呼叫邀请 | `oninviteReceivedByPeer` | [LocalInvitationReceivedByPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationreceivedbypeer)                            |
| 返回给主叫：呼叫邀请已被取消   | `onInviteEndByMyself`    | [LocalInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationcanceled)                                  |
| 返回给主叫：被叫已接收呼叫邀请 | `onInviteAcceptedByPeer` | [LocalInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationaccepted)                                  |
| 返回给主叫：被叫已拒绝呼叫邀请 | `onInviteRefusedByPeer`  | [LocalInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationrefused)                                  |
| 返回给主叫：呼叫邀请过程失败   | `onInviteFailed`         | [LocalInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationfailure)。错误码详见 [LocalInvitationFailureReason](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmstatuscode.localinvitationfailurereason.html)。<sup>6</sup> |


> <sup>6</sup>: 如果呼叫邀请过程已经开始但以失败告终，Agora RTM SDK 会返回 [LocalInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationfailure) 。场景包括：被叫离线，[LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) 对象发送超时 [LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) 过期，或者被叫收到了呼叫邀请但未能在指定时间内响应呼叫邀请。

| 事件                               | 信令                | RTM 实时消息                                                 |
| ---------------------------------- | ------------------- | ------------------------------------------------------------ |
| 返回给 DTMF 用户：收到一个呼叫邀请 | `onInviteMsg`       | N/A                                                          |
| 返回给被叫：收到一个呼叫邀请       | `oninviteReceived`  | [RemoteInvitationReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#remoteinvitationreceived)                                 |
| 返回给被叫：主叫已取消呼叫邀请     | `onInviteEndByPeer` | [RemoteInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html#remoteinvitationcanceled)                                 |
| 返回给被叫：已成功接受呼叫邀请     | N/A                 | [RemoteInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html#remoteinvitationaccepted)                               |
| 返回给被叫：已拒绝呼叫邀请         | N/A                 | [RemoteInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationrefused)                                 |
| 返回给被叫：呼叫邀请过程失败       | N/A                 | [RemoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html#remoteinvitationfailure)。错误码详见 [RemoteInvitationFailureReason](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmstatuscode.remoteinvitationfailurereason.html)。<sup>7</sup> |
|                                    |                     |                                                              |

> <sup>7</sup> 如果呼叫邀请进程已经开始但以失败告终，Agora RTM SDK 会返回 [onRemoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html#remoteinvitationfailure) 回调给被叫。通用场景包括：[RemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html) 发送超时或 [RemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html)  过期。 

## 更新 Token



| 方法           | 信令 | RTM 实时消息 |
| -------------- | ---- | ------------ |
| 更新当前 Token | N/A  | [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#renewtoken) |



| 事件             | 信令 | RTM 实时消息            |
| ---------------- | ---- | ----------------------- |
| 返回方法调用结果 | N/A  | Promise |
| Token 已过期     | N/A  | [TokenExpired](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#tokenexpired)        |
