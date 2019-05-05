
---
title: 限制条件
description: 
platform: iOS,macOS
updatedAt: Sun May 05 2019 05:01:32 GMT+0800 (CST)
---
# 限制条件
本页介绍 Agora RTM Objective-C SDK for iOS/macOS 的使用限制条件。


## 多实例限制

最多支持同时创建 20 个 `AgoraRtmChannel` 实例。若在 `AgoraRtmChannel` 实例达到20 个上限时再调用 `createChannelWithId:delegate` 方法创建频道，SDK 会返回 `nil` 。

## 调用频率限制

<style> table th:first-of-type {     width: 170px; } th:third-of-type {     width: 100px; }</style>

| 功能                                                  | 函数                                                      | 调用频率                |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| 登录到 Agora RTM 系统                              | [loginByToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | 2 次／秒         |
| 发送消息 (点对点和频道消息一并计算在内) | [sendMessage:toPeer:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:) 和 [sendMessage:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:)  | 60 次／秒          |
| 获取频道成员列表                    | [getMembersWithCompletion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) | 每 2 秒 5 次 |
| 更新 token | [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) | 2 次 / 秒 |
| 查询指定用户在线状态 | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) | 每 5 秒 10 次 |

## 字符串长度限制

- 点对点或频道消息的字符串最大长度为 32 KB。详见：[AgoraRtmMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) 。
- 呼叫邀请内容的字符串最大长度为 8 KB. 详见：  [AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 。
- 呼叫邀请响应的字符串最大长度为 8 KB. 详见： [AgoraRtmRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) 。

## 编码格式

仅支持发送 UTF-8 编码格式的频道消息和点对点消息、呼叫邀请 content 和呼叫邀请 response。


## 其他 

- 当频道人数超过 512 人时，用户上下线提示会被自动关闭。如有特殊要求，请请拨打 400 632 6626 或邮件 sales@agora.io。
- v0.9.2 仅支持查询最多 256 个用户的在线状态。
