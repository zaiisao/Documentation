
---
title: 信令 与 RTM 功能对照表
description: 
platform: Linux CPP
updatedAt: Thu Aug 15 2019 10:09:56 GMT+0800 (CST)
---
# 信令 与 RTM 功能对照表
本页对比老信令与 Agora RTM SDK v1.0 的区别。

## 登录登出

| 方法         | 信令                                   | RTM 实时消息                         |
| ------------ | -------------------------------------- | ------------------------------------ |
| 创建实例     | `getInstance`/`createAgoraSDKInstance` | [createRtmService](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1base_1_1_i_agora_service.html#ae3dae3b461aed1b826e5162479530ff1)<sup>1</sup>       |
| 设置回调对象 | `callbackSet`                          | N/A                                  |
| 登录         | `login`/`login2`                       | [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2433a0babbed76ab87084d131227346b)<sup>2</sup>                  |
| 登出         | `logout`                               | [logout](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a24feb518a0bd4c22133f6d42f5a84d03)                             |
| 获取登录状态 | `getStatus`                            | N/A。详见 [onConnectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#aa2e25e87c6f06cfd71b3538786d23743) |
| 销毁实例     | `destroy`                              | [release](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a6c1cf5a9f13640a4bbaaa4fdd2545200)                            |

| 事件         | 信令                  | RTM 实时消息               |
| ------------ | --------------------- | -------------------------- |
| 登录成功     | `onLoginSuccess`      | [onLoginSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a8cf1b2be30172004f595484e0a194d76)           |
| 登录失败     | `onLoginFailed`       | [onLoginFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a4070957075f00a9a54ed60290838fec4)           |
| 登出结果     | `onLogout`            | [onLogout](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a63250f876c60b52d278b43b8b2f0b1de)                 |
| 连接状态改变 | N/A。详见 `getStatus` | [onConnectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#aa2e25e87c6f06cfd71b3538786d23743) |

> - 若无特别说明，Agora RTM Android SDK 的所有核心 API 都应在调用 [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2433a0babbed76ab87084d131227346b) 方法成功并收到 [onLoginSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a8cf1b2be30172004f595484e0a194d76) 回调后调用。Agora Signaling SDK 只允许你每次进入一个频道。
> - <sup>1</sup> 你可以通过调用 [createRtmService](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1base_1_1_i_agora_service.html#ae3dae3b461aed1b826e5162479530ff1) 方法创建多个 IRtmService 实例。Agora RTM SDK 不会限制你创建 IRtmService 实例的个数，但某个 IRtmService 实例最多只能同时加入 20 个 IChannel 频道。
> - <sup>2</sup> RTM 的 Token 生成方式与老信令的 Token 生成方式完全不同。详见[校验用户权限](../../cn/Real-time-Messaging/RTM_key.md)。
> - <sup>2</sup> 信令 Token 采用的 "\_no\_need\_token" 机制不适用于 RTM Token。 
> - <sup>2</sup> Agora RTM SDK 连接或重连 Agora RTM 系统的方式也完全不同。详情请见[连接状态管理](../../cn/Real-time-Messaging/RTM_reconnecting_cpp.md)。 

## 发送点对点消息

| 方法           | 信令                 | RTM 实时消息                |
| -------------- | -------------------- | --------------------------- |
| 创建消息实例   | N/A                  | [createMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#acbbfe84bc22fd161ec5dc4fe098a59ce)<sup>1</sup> |
| 发送点对点消息 | `messageInstantSend` | [sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a08c1b3d444af5a2778ede48e4c677a52)         |

| 事件               | 信令                      | RTM 实时消息                      |
| ------------------ | ------------------------- | --------------------------------- |
| 点对点消息发送成功 | `onMessageSendSuccess`    | [onSendMessageResult](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a533a658eac8e2567df79478fb1041c5c)<sup>2</sup> |
| 点对点消息发送失败 | `onMessageSendError`      | [onSendMessageResult](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a533a658eac8e2567df79478fb1041c5c)             |
| 收到一条点对点消息 | `onMessageInstantReceive` | [onMessageReceivedFromPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#afaf1e899bc3d39dbe33f7fa769897c9a)       |

> <sup>1</sup> 使用 Agora RTM SDK 发送消息之前你必须创建一个消息实例。消息实例既适用于点对点消息也适用于频道消息。Agora RTM SDK 自版本 v0.9.3 起支持通过设置 [SendMessageOptions](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/structagora_1_1rtm_1_1_send_message_options.html) 发送点对点的离线消息。
>
> <sup>2</sup> Agora Signaling SDK 会在服务端收到点对点消息时返回 `onMessageSendSuccess` 回调而 Agora RTM SDK 会在指定用户收到点对点消息后返回 [onSendMessageResult](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a533a658eac8e2567df79478fb1041c5c) 回调。

## 查询指定用户的在线状态

| 方法                   | 信令              | RTM 实时消息             |
| ---------------------- | ----------------- | ------------------------ |
| 查询指定用户的在线状态 | `queryuserStatus` | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3add0055c4455dc8d04bfc37edfd8e94) |



| 事件         | 信令                      | RTM 实时消息                     |
| ------------ | ------------------------- | -------------------------------- |
| 返回查询结果 | `OnQueryUserStatusResult` | [onQueryPeersOnlineStatusResult](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a782b4623d4dcbac5c99fd6a12c42f578) |

> Agora RTM SDK 允许你查询一组用户的在线状态，而不只一个用户的在线状态。

## 用户属性相关操作

| 方法                       | 信令             | RTM 实时消息                          |
| -------------------------- | ---------------- | ------------------------------------- |
| 设置本地用户属性           | `setAttr`        | `addOrUpdateLocalUserAttributes`      |
| 获取本地用户的某个属性     | `getAttr`        | `getUserAttributesBykeys`<sup>1</sup> |
| 获取本地用户的全部属性     | `getAttrAll`     | `getUserAttributes`<sup>2</sup>       |
| 获取指定用户的某个属性     | `getUserAttrAll` | `getUserAttributes`                   |
| 全量替换本地用户的全部属性 | N/A              | `setLocalUserAttributes`              |
| 删除本地用户的指定属性     | N/A              | `deleteLocaluserAttributeByKeys`      |
| 清空本地用户全部属性       | N/A              | `clearLocalUserAttributes`            |
|                            |                  |                                       |

| 事件                   | 信令                    | RTM 实时消息                                                 |
| ---------------------- | ----------------------- | ------------------------------------------------------------ |
| 返回用户属性相关的结果 | `onUserAttributeResult` | <li>`onSetLocalUserAttributesResult` <li> `onAddorUpdateLocalUserAttributesResult` <li> `onDeleteLocalUserAttributesResult` <li> `onClearLocalUserAttributesResult` <li> `onGetUserAttributesResult` |

> - <sup>1</sup> `getuserAttributesBykeys` 方法允许你获取本地用户的一系列属性。
> - <sup>2</sup> `getUserAttributes` 方法允许你获取本地用户或指定远端用户的用户属性。

## 加入离开频道相关

| 方法         | 信令           | RTM 实时消息                |
| ------------ | -------------- | --------------------------- |
| 创建频道实例 | N/A            | `createChannel`<sup>1</sup> |
| 加入指定频道 | `channelJoin`  | `join`<sup>2</sup>          |
| 离开频道     | `channelLeave` | `leave`                     |

| 事件                       | 信令                  | RTM 实时消息     |
| -------------------------- | --------------------- | ---------------- |
| 成功加入指定频道           | `onChannelJoined`     | `onJoinSuccess`  |
| 加入指定频道失败           | `onChannelJoinFailed` | `onJoinFailure`  |
| 远端用户加入当前频道       | `onChannelUserJoined` | `onMemberJoined` |
| 成功离开当前频道           | `onChannelLeaved`     | `onLeave`        |
| 远端频道成员离开当前频道   | `onChannelUserLeaved` | `onMemberLeft`   |
| 加入频道时返回频道成员列表 | `onChannelUserList`   | N/A<sup>3</sup>  |
|                            |                       |                  |

> - <sup>1</sup> Agora RTM SDK 要求你在加入频道前先创建频道实例。
> - <sup>2</sup> Agora RTM SDK 允许你最多同时加入 20 个频道。 
> - <sup>3</sup> Agora RTM SDK 不会在用户成功加入频道时返回当前频道成员列表。
> - Agora RTM SDK 不支持特殊频道功能。

## 频道消息

| 方法                 | 信令                      | RTM 实时消息                |
| -------------------- | ------------------------- | --------------------------- |
| 创建消息实例         | N/A                       | `createMessage`<sup>1</sup> |
| 从频道内发送频道消息 | `messageChannelSend`      | `sendMessage`<sup>2</sup>   |
| 从频道外发送频道消息 | `messageChannelSendForce` | N/A                         |



| 事件             | 信令                      | RTM 实时消息                    |
| ---------------- | ------------------------- | ------------------------------- |
| 频道消息发送成功 | `onMessageSendSuccess`    | `onSendMessageResult`           |
| 频道消息         | `onMessageSendError`      | `onSendMessageResult`           |
| 收到一条频道消息 | `onMessageChannelReceive` | `onMessageReceived`<sup>3</sup> |



> - <sup>1</sup> Agora RTM SDK 要求你在发送频道消息或点对点消息之前必须创建一个消息实例。
> - <sup>2</sup> Agora RTM SDK 暂不支持从频道外发送频道消息。也就是说，你必须加入频道才能向其他频道成员发送频道消息。
> - <sup>3</sup> 与 Agora Signaling SDK 的行为不同，`onMessageReceived` 回调返回给频道内的其他频道成员，而非返回给发送人。

## 频道属性相关

| 方法                 | 信令               | RTM 实时消息 |
| -------------------- | ------------------ | ------------ |
| 设置一条频道属性     | `channelSetAttr`   | N/A          |
| 删除一条频道属性     | `channelDelAttr`   | N/A          |
| 删除频道的全部属性。 | `channelClearAttr` | N/A          |

> Agora RTM SDK v1.1 将支持频道属性相关操作。

| 事件           | 信令                   | RTM 实时消息 |
| -------------- | ---------------------- | ------------ |
| 频道属性已更新 | `onChannelAttrUpdated` | N/A          |



## 获取当前频道用户列表

| 方法                   | 信令     | RTM 实时消息             |
| ---------------------- | -------- | ------------------------ |
| 获取指定频道的成员列表 | `invoke` | `getMembers`<sup>1</sup> |



| 事件                   | 信令          | RTM 实时消息   |
| ---------------------- | ------------- | -------------- |
| 返回指定频道的成员列表 | `onInvokeRet` | `onGetMembers` |

> <sup>1</sup> 你必须加入一个 RtmChannel 频道后才能获取该频道的成员列表。当频道人员超过 512 人上限，Agora RTM SDK 会随机返回频道内的 512 个当前频道成员。 



## 获取制定频道用户人数

| 方法                 | 信令                  | RTM 实时消息 |
| -------------------- | --------------------- | ------------ |
| 获取指定频道成员人数 | `channelQueryUserNum` | N/A          |



| 事件                   | 信令                          | RTM 实时消息 |
| ---------------------- | ----------------------------- | ------------ |
| 返回指定频道的用户人数 | `onChannelQueryUserNumResult` | N/A          |

> Agora RTM SDK v1.1 将支持该功能。 

## 呼叫邀请



| 方法                                          | 信令                                     | RTM 实时消息                            |
| --------------------------------------------- | ---------------------------------------- | --------------------------------------- |
| 创建 RTM 呼叫管理器                           | N/A                                      | `getRtmCallManager`<sup>1</sup>         |
| 供主叫创建并管理一个 `LocalInvitation` 实例。 | N/A                                      | `createLocalCallInvitation`<sup>2</sup> |
| 供主叫向指定用户（被叫）发送呼叫邀请          | `channelInviteUser`/`channelInviteUser2` | `sendLocalInvitation`<sup>3</sup>   |
| 供主叫发送 DTMF 呼叫邀请。                    | `channelInviteDTMF`                      | N/A                                     |
| 供主叫取消一个发出的呼叫邀请                  | `channelInviteEnd`                       | `cancelLocalInvitation`<sup>4</sup>     |
| 供被叫接收一个呼叫邀请                        | `channelInviteAccept`                    | `acceptRemoteInvitation`                |
| 供被叫拒绝一个呼叫邀请                        | `channelInviteRefuse`                    | `refuseRemoteInvitation`                |

> - <sup>1</sup> Agora RTM SDK 要求主叫或被叫在发送、取消、接收或拒绝一个呼叫邀请前必须创建一个 `IRtmCallManager` 实例。
> - <sup>2</sup> Agora RTM SDK 引入了 `ILocalInvitation` 和 `IRemoteInvitation` 对象。前者由主叫通过调用 `createLocalCallInvitation` 方法创建，后者在被叫收到呼叫邀请时由 SDK 自动创建。你可以将这两个对象理解为同一个呼叫邀请的两种不同形式。主叫通过 `ILocalInvitation` 对象指定被叫，设置自定义内容或检查 `ILocalInvitationState` 状态，被叫通过 `IRemoteInvitation` 对象设置响应内容，检查主叫 ID，或者检查 `IRemoteInvitationState` 状态。
> - <sup>3</sup> `sendLocalInvitation` 函数不带 `channelInviteUser2` 函数中的 `extra` 参数。 
> - <sup>4</sup> `channelInviteEnd` 方法可以在任意时间取消一个呼叫邀请，而 `cancelLocalInvitation` 方法只能取消一个已发送的正在进行的呼叫邀请。
> - 为了和 Agora Signaling SDK 互通，你必须把你的 Agora RTM SDK 版本升级到 v1.0 以上并设置 channel ID。请注意即使被叫接收了呼叫邀请，Agora RTM SDK 也不会将主叫或被叫加入指定频道。
> 如果用户在 `LocalInvitation` 生命周期开始之前或生命周期结束之后调用了 `sendLocalInvitation`、 `cancelLocalInvitation`、 `acceptRemoteInvitation` 或 `refuseRemoteInvitation` ，Agora RTM SDK 会返回 `INVITATION_API_CALL_ERR_CODE` 错误码。



| 事件                           | 信令                     | RTM 实时消息                                                 |
| ------------------------------ | ------------------------ | ------------------------------------------------------------ |
| 返回给主叫：被叫已收到呼叫邀请 | `oninviteReceivedByPeer` | `onLocalInvitationReceivedByPeer`                            |
| 返回给主叫：呼叫邀请已被取消   | `onInviteEndByMyself`    | `onLocalInvitationCanceled`                                  |
| 返回给主叫：被叫已接收呼叫邀请 | `onInviteAcceptedByPeer` | `onLocalInvitationAccepted`                                  |
| 返回给主叫：被叫已拒绝呼叫邀请 | `onInviteRefusedByPeer`  | `onLocalInvitationRefused`                                   |
| 返回给主叫：呼叫邀请过程失败   | `onInviteFailed`         | `onLocalInvitationFailure`。错误码详见 `LOCAL_INVITATION_ERR_CODE`。<sup>5</sup> |

> <sup>5</sup>: 如果呼叫邀请过程已经开始但以失败告终，Agora RTM SDK 会返回 `onLocalInvitationFailure` 。场景包括：被叫离线，`ILocalCallInvitation` 对象发送超时 `ILocalCallInvitation`过期，或者被叫收到了呼叫邀请但未能在指定时间内响应呼叫邀请。

| 事件                               | 信令                | RTM 实时消息                                                 |
| ---------------------------------- | ------------------- | ------------------------------------------------------------ |
| 返回给 DTMF 用户：收到一个呼叫邀请 | `onInviteMsg`       | N/A                                                          |
| 返回给被叫：收到一个呼叫邀请       | `oninviteReceived`  | `onRemoteInvitationReceived`                                 |
| 返回给被叫：主叫已取消呼叫邀请     | `onInviteEndByPeer` | `onRemoteInvitationCanceled`                                 |
| 返回给被叫：已成功接受呼叫邀请     | N/A                 | `onRemoteInvitationAccepted`                                 |
| 返回给被叫：已拒绝呼叫邀请         | N/A                 | `onRemoteInvitationRefused`                                  |
| 返回给被叫：呼叫邀请过程失败       | N/A                 | `onRemoteInvitationFailure`。错误码详见 `REMOTE_INVITATION_ERR_CODE`。<sup>6</sup> |

> <sup>6</sup> 如果呼叫邀请进程已经开始但以失败告终，Agora RTM SDK 会返回 `onRemoteInvitationFailure` 回调给被叫。通用场景包括：`IRemoteCallInvitation` 发送超时或 `IRemoteCallInvitation`  过期。 

## 更新 Token



| 方法           | 信令 | RTM 实时消息 |
| -------------- | ---- | ------------ |
| 更新当前 Token | N/A  | `renewToken` |



| 事件             | 信令 | RTM 实时消息            |
| ---------------- | ---- | ----------------------- |
| 返回方法调用结果 | N/A  | `onRenewTokenResult` |
| Token 已过期     | N/A  | `onTokenExpired`        |
