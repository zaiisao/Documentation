
---
title: 限制条件
description: 
platform: iOS,macOS
updatedAt: Sun Jul 28 2019 14:42:06 GMT+0800 (CST)
---
# 限制条件
本页介绍 Agora RTM Objective-C SDK for iOS/macOS 的使用限制条件。


## 调用频率限制

<style> table th:first-of-type {     width: 170px; } th:third-of-type {     width: 100px; }</style>

| 功能                                                  | 方法                                                      | 调用频率上限                |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| 登录到 Agora RTM 系统                              | [loginByToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | 2 次／秒         |
| 发送消息 (点对点和频道消息一并计算) | <li>[sendMessage:toPeer:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:) <li>[sendMessage:toPeer:sendMessageOptions:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:) <li> [sendMessage:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:)  | 60 次／秒          |
| 获取频道成员列表                    | [getMembersWithCompletion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) | 每 2 秒 5 次 |
| 更新 token | [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) | 2 次／秒 |
| 查询指定用户在线状态 | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) | 每 5 秒 10 次 |
| 用户属性增删修改(一并计算）| <li>[setLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLocalUserAttributes:completion:)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateLocalUserAttributes:completion:)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteLocalUserAttributesByKeys:completion:)<li>[clearLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearLocalUserAttributesWithCompletion:) | 每 5 秒 10 次          |
| 用户属性查询(一并计算）| <li>[getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)<li>[getUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:) | 每 5 秒 40 次          |

## 字符串长度限制

- 点对点或频道消息的字符串最大长度为 32 KB。详见：[AgoraRtmMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) 。
- 呼叫邀请内容的字符串最大长度为 8 KB. 详见：  [AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 。
- 呼叫邀请响应的字符串最大长度为 8 KB. 详见： [AgoraRtmRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) 。

## 编码格式限制

仅支持发送 UTF-8 编码格式的频道消息和点对点消息、呼叫邀请 content、呼叫邀请 response。

## 其他 


- 当频道人数超过 512 人时，用户上下线提示会被自动关闭。如有特殊要求，请拨打 400 632 6626 或邮件 sales@agora.io。
- 当前版本仅支持查询最多 256 个用户的在线状态。
- 单次属性设置的最大值为 16 KB，单次属性操作设置的属性条目不能超过 32 个。
