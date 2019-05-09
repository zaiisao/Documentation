
---
title: 限制条件
description: RTM Linux Java SDK limitations.
platform: Linux Java
updatedAt: Thu May 09 2019 02:38:03 GMT+0800 (CST)
---
# 限制条件
本页面提供 Agora RTM Java SDK for Android 的使用限制条件。

## 多实例限制

最多支持同时创建 20 个 `RtmChannel` 实例。若在 `RtmChannel` 实例达到20 个上限时再调用 `createChannel()` 方法创建频道，SDK 会抛出异常。我们强烈推荐你在不用某个 `RtmChannel` 实例时调用  `RtmChannel.release()` 方法彻底释放其资源。

## 调用频率限制

<style> table th:first-of-type {     width: 170px; } th:third-of-type {     width: 100px; }</style>

| 功能                                    | 函数                                                         | 调用频率     |
| --------------------------------------- | ------------------------------------------------------------ | ------------ |
| 登录到 Agora RTM 系统                   | [RtmClient.login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) | 2 次／秒     |
| 发送消息 (点对点和频道消息一并计算在内) | [RtmClient.sendMessageToPeer()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a) 和 [RtmChannel.SendMessage()](https://docs.agora.io/cn/Real-time-Messaging/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a57087adf4227a17c774ea292840148a0) | 60 次／秒    |
| 获取频道成员列表                        | [RtmChannel.getMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) | 每 2 秒 5 次 |

## 字符串长度限制

- 点对点或频道消息的字符串最大长度为 32 KB。详见： [RtmMessage.setText()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_javaclassio_1_1agora_1_1rtm_1_1_rtm_message.html#a114bf5f4d728e1a5e31792491bf4a1d2) 。
- 呼叫邀请内容的字符串最大长度为 8 KB. 详见： [LocalInvitation.setContent()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a4cec28ff6d356242329b1034c7531445) 。
- 呼叫邀请响应的字符串最大长度为 8 KB. 详见： [RemoteInvitation.setResponse()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a229b8cf773eaa0e79b0d67815fd6b6f1) 。

## 编码格式

仅支持发送 UTF-8 编码格式的频道消息和点对点消息、呼叫邀请 content、呼叫邀请 response。

## 其他 

- Agora RTM SDK 目前与 Agora Signaling SDK 暂未实现互通。互通功能将于近期实现。
- 当频道人数超过 512 人时，用户上下线提示会被自动关闭。如有特殊要求，请请拨打 400 632 6626 或邮件 sales@agora.io。
- 当前版本仅支持查询最多 256 个用户的在线状态。
