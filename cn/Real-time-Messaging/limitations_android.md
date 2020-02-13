
---
title: 限制条件
description: 
platform: Android
updatedAt: Thu Feb 13 2020 09:24:26 GMT+0800 (CST)
---
# 限制条件

本页简要介绍 Agora RTM Java SDK for Android 的使用限制条件，包括最大调用频率、最大字符串长度、编码格式等。



## 调用频率限制

所有的 qps 都是针对单个 RtmClient 实例而言，而非针对单个 Agora RTM SDK。

<div class="alert note">你可以通过创建多实例提高 API 的调用频率。</div>

<style> table th:first-of-type {     width: 170px; } th:third-of-type {     width: 100px; }</style>

| 功能                                                  | 方法                                                      | 调用频率上限                |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| 登录到 Agora RTM 系统                              | [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) | 2 次／秒         |
| 查询单个或多个频道的成员人数 | [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aff0384f2a004ed75498e20e1917352e4) | 1 次／秒 |
| 加入频道<sup>1</sup> | [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40) | 每 3 秒 50 次 |
| 发送消息 (点对点和频道消息一并计算) | <li>[sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a) <li>[sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9)<li>[sendMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a57087adf4227a17c774ea292840148a0) | 60 次／秒          |
| 获取频道成员列表                    | [getMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) | 每 2 秒 5 次 |
| 更新 token| [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353) | 2 次／秒 |
| 查询指定用户在线状态 | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3) | 每 5 秒 10 次 |
| 用户属性增删修改(一并计算）| <li>[setLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a339b7b2371ff2b86137b6db6c1c66294)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a765b186d62ed3ef6d67a5e875b040875)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a2477533989c1bb9ced831af210f1dba4)<li>[clearLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ae0c6c5c5bae6020e69009441d8a41785) | 每 5 秒 10 次          |
| 用户属性查询(一并计算）| <li>[getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755)<li>[getUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193) | 每 5 秒 40 次          |
| 频道属性增删修改(一并计算）| <li>[setChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ad25f51a3671db50e348ec6c170044ec6)<li>[addOrUpdateChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a997a31e6bfe1edc9b6ef58a931ef3f23)<li>[deleteChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a4cbf3329abda4940b73a75455cd1dc06)<li>[clearChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6ed0ef4baacda8fa00eda5373d17f59f) | 每 5 秒 10 次          |
| 频道属性查询(一并计算）| <li>[getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a81f14a747a4012815ab4ba8d9e480fb6)<li>[getChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a358b47f4b42d678fafa76f3f30290e5e) | 每 5 秒 10 次          |
| 订阅指定单个或多个用户的在线状态   | [subscribePeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a7a9ec7398c013ed35e17bc5d93e71420) | 每 5 秒 10 次 |
| 退订指定单个或多个用户的在线状态    | [unSubscribePeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#acf3ab093be17a0752d8aff094e3aabc4) | 每 5 秒 10 次 |
| 获取某特定内容被订阅的用户列表   | [queryPeersBySubscriptionOption](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a971c357f7d0c27d122ff877389314ccc) | 每 5 秒 10 次 |

> <sup>1</sup> 加入相同频道的频率限制为每 5 秒 2 次。

## 字符串长度限制

- 点对点或频道消息的字符串最大长度为 32 KB。详见： [RtmMessage.setText](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a114bf5f4d728e1a5e31792491bf4a1d2) 。
- 呼叫邀请内容的字符串最大长度为 8 KB. 详见： [LocalInvitation.setContent](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a4cec28ff6d356242329b1034c7531445) 。
- 呼叫邀请响应的字符串最大长度为 8 KB. 详见： [RemoteInvitation.setResponse](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a229b8cf773eaa0e79b0d67815fd6b6f1) 。


## 编码格式限制

仅支持发送 UTF-8 编码格式的频道消息和点对点消息、呼叫邀请 content、呼叫邀请 response。

## 其他 


- 当频道人数超过 512 人时，用户进出频道提示会被自动关闭。
- 支持单次查询最多 256 个用户的在线状态。
- 单次用户属性设置的最大值为 16 KB，单次频道属性设置的最大值为 32 KB，单条用户或频道属性的最大值为 8 KB，单次属性操作设置的属性条目不能超过 32 个。


