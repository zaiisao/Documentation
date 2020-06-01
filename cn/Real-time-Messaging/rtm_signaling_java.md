
---
title: 信令 与 RTM 功能对照表
description: 
platform: Linux Java
updatedAt: Mon Jun 01 2020 02:47:52 GMT+0800 (CST)
---
# 信令 与 RTM 功能对照表
本页对比 Agora Signaling SDK 与 Agora RTM SDK 的区别。

## 登录登出

| 方法         | 信令                                   | 实时消息                          |
| ------------ | -------------------------------------- | ------------------------------------- |
| 创建实例     | `getInstance`/`createAgoraSDKInstance` | [createInstance](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a0bd5585641a955cbb54f279a1dae55df)<sup>1</sup>          |
| 设置回调对象 | `callbackSet`                          | N/A                                   |
| 登录         | `login`/`login2`                       | [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a)<sup>2</sup>                   |
| 登出         | `logout`                               | [logout](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6f5695854e251ddd4ba05547ab47b317)                              |
| 获取登录状态 | `getStatus`                            | N/A。详见 [onConnectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#a9b6f86cb2d7d5ec4adf0b6d645c16bf9) |
| 销毁实例     | `destroy`                              | [release](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a5147d00d14afeebf0926b0d2f01079f5)                             |

| 事件         | 信令                   | 实时消息               |
| ------------ | ---------------------- | -------------------------- |
| 登录成功     | `onLoginSuccess`       | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)                |
| 登录失败     | `onLoginFailed`        | [onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)                |
| 登出结果     | `onLogout`             | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5) 或 [onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) |
| 连接状态改变 | N/A。详见 `getStatus` | [onConnectionStateChanged](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#a9b6f86cb2d7d5ec4adf0b6d645c16bf9) |

> - 若无特别说明，Agora RTM Android SDK 的所有核心 API 都应在调用 [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) 方法成功并收到 [onSuccess](../../cn/Real-time-Messaging/rtm_signaling_java.md) 回调后调用。Agora Signaling SDK 只允许你每次进入一个频道。
> - <sup>1</sup> 你可以通过调用 [createInstance](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a0bd5585641a955cbb54f279a1dae55df) 方法创建多个 RtmClient 实例。Agora RTM SDK 不会限制你创建 RtmClient 实例的个数，但某个 RtmClient 最多只能同时加入 20 个 RtmChannel 频道。
> - <sup>2</sup> RTM 的 Token 生成方式与老信令的 Token 生成方式完全不同。详见[校验用户权限](../../cn/Real-time-Messaging/rtm_token.md)。
> - <sup>2</sup> 信令 Token 采用的 "\_no\_need\_token" 机制不适用于 RTM Token。 
> - <sup>2</sup> Agora RTM SDK 连接或重连 Agora RTM 系统的方式也完全不同。详情请见[连接状态管理](../../cn/Real-time-Messaging/reconnecting_java.md)。 

## 发送点对点消息

| 方法           | 信令                 | 实时消息                |
| -------------- | -------------------- | --------------------------- |
| 创建消息实例   | N/A                  | [createMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a77dbd15cb6c9db3844fb313bd5dceac3)<sup>1</sup> |
| 发送点对点消息 | `messageInstantSend` | [sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9)         |

| 事件               | 信令                      | 实时消息            |
| ------------------ | ------------------------- | ----------------------- |
| 点对点消息发送成功 | `onMessageSendSuccess`    | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)<sup>2</sup> |
| 点对点消息发送失败 | `onMessageSendError`      | [onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)             |
| 收到一条点对点消息 | `onMessageInstantReceive` | [onMessageReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#af760814981718fb31d88acb8251d19b6)     |

> <sup>1</sup> 使用 Agora RTM SDK 发送消息之前你必须创建一个消息实例。消息实例既适用于点对点消息也适用于频道消息。Agora RTM SDK 自版本 v0.9.3 起支持通过设置 [sendMessageOptions](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_send_message_options.html) 发送点对点的离线消息。
>
> <sup>2</sup> Agora Signaling SDK 会在服务端收到点对点消息时返回 `onMessageSendSuccess` 回调而 Agora RTM SDK 会在指定用户收到点对点消息后返回 [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5) 回调。

## 查询指定用户的在线状态

| 方法                   | 信令              | 实时消息             |
| ---------------------- | ----------------- | ------------------------ |
| 查询指定用户的在线状态 | `queryuserStatus` | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3) |



| 事件         | 信令                      | 实时消息 |
| ------------ | ------------------------- | ------------ |
| 返回查询结果 | `OnQueryUserStatusResult` | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)  |

> Agora RTM SDK 允许你查询一组用户的在线状态，而不只一个用户的在线状态。

## 用户属性相关操作

| 方法                       | 信令             | 实时消息                          |
| -------------------------- | ---------------- | ------------------------------------- |
| 设置本地用户属性           | `setAttr`        | [addOrUpdateLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a765b186d62ed3ef6d67a5e875b040875)      |
| 获取本地用户的某个属性     | `getAttr`        | [getUserAttributesBykeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193)<sup>1</sup> |
| 获取本地用户的全部属性     | `getAttrAll`     | [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755)<sup>2</sup>       |
| 获取指定用户的某个属性     | `getUserAttrAll` | [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755)                   |
| 全量替换本地用户的全部属性 | N/A              | [setLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a339b7b2371ff2b86137b6db6c1c66294)              |
| 删除本地用户的指定属性     | N/A              | [deleteLocaluserAttributeByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a2477533989c1bb9ced831af210f1dba4)     |
| 清空本地用户全部属性       | N/A              | [clearLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ae0c6c5c5bae6020e69009441d8a41785)            |
|                            |                  |                                       |

| 事件                   | 信令                    | 实时消息            |
| ---------------------- | ----------------------- | ----------------------- |
| 返回用户属性相关的结果 | `onUserAttributeResult` | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) |

> - <sup>1</sup> [getuserAttributesBykeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193) 方法允许你获取本地用户的一系列属性。
> - <sup>2</sup> [getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755) 方法允许你获取本地用户或指定远端用户的用户属性。

## 加入离开频道相关

| 方法         | 信令           | 实时消息                |
| ------------ | -------------- | --------------------------- |
| 创建频道实例 | N/A            | [createChannel](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a)<sup>1</sup> |
| 加入指定频道 | `channelJoin`  | [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40)<sup>2</sup>          |
| 离开频道     | `channelLeave` | [leave](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a9e0b6aad17bfceb3c9c939351a467d14)                     |

| 事件                       | 信令                  | 实时消息     |
| -------------------------- | --------------------- | ---------------- |
| 成功加入指定频道           | `onChannelJoined`     | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)      |
| 加入指定频道失败           | `onChannelJoinFailed` | [onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)      |
| 远端用户加入当前频道       | `onChannelUserJoined` | [onMemberJoined](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a9d39a66a0a17ebbb6c8e5c5e520100dc) |
| 成功离开当前频道           | `onChannelLeaved`     | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)      |
| 远端频道成员离开当前频道   | `onChannelUserLeaved` | [onMemberLeft](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#acc14e332fefe76d16571c14665c2cb1c)   |
| 加入频道时返回频道成员列表 | `onChannelUserList`   | N/A<sup>3</sup>  |
|                            |                       |                  |

> - <sup>1</sup> Agora RTM SDK 要求你在加入频道前先创建频道实例。
> - <sup>2</sup> Agora RTM SDK 允许你最多同时加入 20 个频道。 
> - <sup>3</sup> Agora RTM SDK 不会在用户成功加入频道时返回当前频道成员列表。
> - Agora RTM SDK 不支持特殊频道功能。

## 频道消息

| 方法                 | 信令                      | 实时消息                |
| -------------------- | ------------------------- | --------------------------- |
| 创建消息实例         | N/A                       | [createMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a77dbd15cb6c9db3844fb313bd5dceac3)<sup>1</sup> |
| 从频道内发送频道消息 | `messageChannelSend`      | [sendMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a57087adf4227a17c774ea292840148a0)<sup>2</sup>   |
| 从频道外发送频道消息 | `messageChannelSendForce` | N/A                         |



| 事件             | 信令                      | 实时消息                    |
| ---------------- | ------------------------- | ------------------------------- |
| 频道消息发送成功 | `onMessageSendSuccess`    | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)                     |
| 频道消息         | `onMessageSendError`      | [onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)                     |
| 收到一条频道消息 | `onMessageChannelReceive` | [onMessageReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a2d527e4040bcabf038533810adbd296c)<sup>3</sup> |



> - <sup>1</sup> Agora RTM SDK 要求你在发送频道消息或点对点消息之前必须创建一个消息实例。
> - <sup>2</sup> Agora RTM SDK 暂不支持从频道外向频道内发送消息。你必须加入频道才能向该频道成员发送频道消息。不过，你也可以通过设置频道属性并在每次 API 调用时开启 enableNotificationToChannelMembers 通知该频道所有成员本次更新。
> - <sup>3</sup> 与 Agora Signaling SDK 的行为不同，[onMessageReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a2d527e4040bcabf038533810adbd296c) 回调返回给频道内的其他频道成员，而非返回给发送人。

## 频道属性相关操作

| 方法                 | 信令               | 实时消息 |
| -------------------- | ------------------ | ------------ |
| 设置一条频道属性     | `channelSetAttr`   | [addOrUpdateChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a997a31e6bfe1edc9b6ef58a931ef3f23)  |
| 删除一条频道属性     | `channelDelAttr`   | [deleteChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a4cbf3329abda4940b73a75455cd1dc06)         |
| 删除频道的全部属性 | `channelClearAttr` | [clearChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6ed0ef4baacda8fa00eda5373d17f59f)          |
| 全量设置某指定频道的属性 | N/A | [setChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ad25f51a3671db50e348ec6c170044ec6)   |
| 获取某指定频道的全部属性 | N/A | [getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a81f14a747a4012815ab4ba8d9e480fb6) |
| 获取某指定频道指定属性名的属性 | N/A | [getChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a358b47f4b42d678fafa76f3f30290e5e) |

| 事件           | 信令                   | 实时消息 |
| -------------- | ---------------------- | ------------ |
| 频道属性已更新 | `onChannelAttrUpdated` | [onAttributesUpdated](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a2904a1f1f78c497b9176fffb853be96f)          |



## 获取当前频道用户列表

| 方法                   | 信令     | 实时消息             |
| ---------------------- | -------- | ------------------------ |
| 获取指定频道的成员列表 | `invoke` | [getMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) |



| 事件                   | 信令          | 实时消息            |
| ---------------------- | ------------- | ----------------------- |
| 返回指定频道的成员列表 | `onInvokeRet` | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) |



## 获取指定频道用户人数

| 方法                 | 信令                  | 实时消息 |
| -------------------- | --------------------- | ------------ |
| 获取指定频道成员人数 | `channelQueryUserNum` | [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aff0384f2a004ed75498e20e1917352e4)          |



| 事件                   | 信令                          | 实时消息 |
| ---------------------- | ----------------------------- | ------------ |
| 返回指定频道的用户人数 | `onChannelQueryUserNumResult` | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)          |


## 呼叫邀请

| 方法                                          | 信令                                     | 实时消息                                                 |
| --------------------------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
| 创建 RTM 呼叫管理器                           | N/A                                      | [getRtmCallManager](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ab3ae053d5617855ddbbdae9149067cfd)<sup>1</sup> |
| 供主叫创建并管理一个 `LocalInvitation` 实例。 | N/A                                      | [createLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a1756dca077267acaa407c6901daa2248)<sup>2</sup> |
| 供主叫向指定用户（被叫）发送呼叫邀请          | `channelInviteUser`/`channelInviteUser2` | [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#af899697061305ca840e829b92c78e353)<sup>3</sup> |
| 供主叫发送 DTMF 呼叫邀请。                    | `channelInviteDTMF`                      | N/A                                                          |
| 供主叫取消一个发出的呼叫邀请                  | `channelInviteEnd`                       | [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f03bfe1cfd6987fbc7b5a4dc484f564)<sup>4</sup> |
| 供被叫接收一个呼叫邀请                        | `channelInviteAccept`                    | [acceptRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f6f97c84e426e2fbd8a5dda71e2fc6c) |
| 供被叫拒绝一个呼叫邀请                        | `channelInviteRefuse`                    | [refuseRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a2ce4af944183976d18c055816f756bf6) |
|                                               |                                          |                                                              |

> - <sup>1</sup> Agora RTM SDK 要求主叫或被叫在发送、取消、接收或拒绝一个呼叫邀请前必须创建一个 [RtmCallManager](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html) 实例。
> - <sup>2</sup> Agora RTM SDK 引入了 [LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) 和 [RemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html) 对象。前者由主叫通过调用 [createLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a1756dca077267acaa407c6901daa2248) 方法创建，后者在被叫收到呼叫邀请时由 SDK 自动创建。你可以将这两个对象理解为同一个呼叫邀请的两种不同形式。主叫通过 [LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) 对象指定被叫，设置自定义内容或检查 [LocalInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_state.html) 状态，被叫通过 [RemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html) 对象设置响应内容，检查主叫 ID，或者检查 [RemoteInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_remote_invitation_state.html) 状态。
> - <sup>3</sup> [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#af899697061305ca840e829b92c78e353) 函数不带 `channelInviteUser2` 函数中的 `extra` 参数。 
> - <sup>4</sup> `channelInviteEnd` 方法可以在任意时间取消一个呼叫邀请，而 [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f03bfe1cfd6987fbc7b5a4dc484f564) 方法只能取消一个已发送的正在进行的呼叫邀请。
> - 为了和 Agora Signaling SDK 互通，你必须把你的 Agora RTM SDK 版本升级到 v1.0 以上并设置 channel ID。请注意即使被叫接收了呼叫邀请，Agora RTM SDK 也不会将主叫或被叫加入指定频道。

| 同步回调     | 信令 | 实时消息                                                 |
| ------------ | ---- | ------------------------------------------------------------ |
| 方法调用成功 | N/A  | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5) |
| 方法调用失败 | N/A  | [onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)。错误码详见 [InvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_invitation_api_call_error.html)<sup>5</sup> |

> <sup>5</sup> 如果用户在 [LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) 生命周期开始之前或生命周期结束之后调用了 [sendLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#af899697061305ca840e829b92c78e353)、 [cancelLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f03bfe1cfd6987fbc7b5a4dc484f564)、 [acceptRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f6f97c84e426e2fbd8a5dda71e2fc6c) 或 [refuseRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a2ce4af944183976d18c055816f756bf6) ，Agora RTM SDK 会返回 [onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) 回调以及 [InvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_invitation_api_call_error.html) 错误码。

| 事件                           | 信令                     | 实时消息                                                 |
| ------------------------------ | ------------------------ | ------------------------------------------------------------ |
| 返回给主叫：被叫已收到呼叫邀请 | `oninviteReceivedByPeer` | [onLocalInvitationReceivedByPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a24e1cb71d3e752963da49bdf91847788) |
| 返回给主叫：呼叫邀请已被取消   | `onInviteEndByMyself`    | [onLocalInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#ae3164e81772cd4d6171165b1705adcaa) |
| 返回给主叫：被叫已接收呼叫邀请 | `onInviteAcceptedByPeer` | [onLocalInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a4dece02a62a187a66c2415fecf6b75dc) |
| 返回给主叫：被叫已拒绝呼叫邀请 | `onInviteRefusedByPeer`  | [onLocalInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6224643c400268d356cb5d489825bdd0) |
| 返回给主叫：呼叫邀请过程失败   | `onInviteFailed`         | [onLocalInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#acfefb97eaca497cbd71a0c1cbf5067b0)。错误码详见 [LocalInvitationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_error.html)。<sup>6</sup> |
|                                |                          |                                                              |

> <sup>6</sup>: 如果呼叫邀请过程已经开始但以失败告终，Agora RTM SDK 会返回 [onLocalInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#acfefb97eaca497cbd71a0c1cbf5067b0) 。场景包括：被叫离线，[LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) 对象发送超时 [LocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html)过期，或者被叫收到了呼叫邀请但未能在指定时间内响应呼叫邀请。

| 事件                               | 信令                | 实时消息                                                 |
| ---------------------------------- | ------------------- | ------------------------------------------------------------ |
| 返回给 DTMF 用户：收到一个呼叫邀请 | `onInviteMsg`       | N/A                                                          |
| 返回给被叫：收到一个呼叫邀请       | `oninviteReceived`  | [onRemoteInvitationReceived](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a8d01498a993c4016aa45ccb9bf4e9097) |
| 返回给被叫：主叫已取消呼叫邀请     | `onInviteEndByPeer` | [onRemoteInvitationCanceled](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#ae3164e81772cd4d6171165b1705adcaa) |
| 返回给被叫：已成功接受呼叫邀请     | N/A                 | [onRemoteInvitationAccepted](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a81d9d3de89d08c41408d8a94c8309d29) |
| 返回给被叫：已拒绝呼叫邀请         | N/A                 | [onRemoteInvitationRefused](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6224643c400268d356cb5d489825bdd0) |
| 返回给被叫：呼叫邀请过程失败       | N/A                 | [onRemoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6f9f2bbbfbcb0a766c6f1b2e4a8314a1)。错误码详见 [RemoteInvitationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_remote_invitation_error.html)。<sup>7</sup> |
|                                    |                     |                                                              |

> <sup>7</sup> 如果呼叫邀请进程已经开始但以失败告终，Agora RTM SDK 会返回 [onRemoteInvitationFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6f9f2bbbfbcb0a766c6f1b2e4a8314a1) 回调给被叫。通用场景包括：[RemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html) 发送超时或 [RemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html)  过期。 

## 更新 Token


| 方法           | 信令 | 实时消息 |
| -------------- | ---- | ------------ |
| 更新当前 Token | N/A  | [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353)  |


| 事件             | 信令 | 实时消息            |
| ---------------- | ---- | ----------------------- |
| 返回方法调用结果 | N/A  | [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) |
| Token 已过期     | N/A  | [onTokenExpired](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#aef74f37ed8797d274115d7f13785134e)        |
