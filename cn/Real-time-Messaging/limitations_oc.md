
---
title: 限制条件
description: 
platform: iOS,macOS
updatedAt: Thu Feb 20 2020 13:32:37 GMT+0800 (CST)
---
# 限制条件

本页简要介绍 Agora RTM Objective-C SDK for iOS 或 Agora RTM Objective-C SDK for macOS 的使用限制条件，包括最大调用频率、最大字符串长度、编码格式等。


## 调用频率限制

所有的 qps 都是针对单个 AgoraRtmKit 实例而言，而非针对单个 Agora RTM SDK。

<div class="alert note">你可以通过创建多实例提高 API 的调用频率。</div>

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| 功能                                                  | 方法                                                      | 调用频率上限                |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| 登录到 Agora RTM 系统                              | [loginByToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | 2 次／秒         |
| 查询单个或多个频道的成员人数 | [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelMemberCount:completion:) | 1 次／秒 |
| 加入频道<sup>1</sup> | [joinWithCompletion](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) | 每 3 秒 50 次 |
| 发送消息 (点对点和频道消息一并计算) | <li>[sendMessage:toPeer:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:) <li>[sendMessage:toPeer:sendMessageOptions:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:) <li> [sendMessage:completion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:)  | 60 次／秒          |
| 获取频道成员列表                    | [getMembersWithCompletion:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) | 每 2 秒 5 次 |
| 更新 token | [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) | 2 次／秒 |
| 查询指定用户在线状态 | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) | 每 5 秒 10 次 |
| 用户属性增删修改(一并计算）| <li>[setLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLocalUserAttributes:completion:)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateLocalUserAttributes:completion:)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteLocalUserAttributesByKeys:completion:)<li>[clearLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearLocalUserAttributesWithCompletion:) | 每 5 秒 10 次          |
| 用户属性查询(一并计算）| <li>[getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)<li>[getUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:) | 每 5 秒 40 次          |
| 频道属性增删修改(一并计算）| <li>[setChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setChannel:Attributes:Options:completion:)<li>[addOrUpdateChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateChannel:Attributes:Options:completion:)<li>[deleteChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteChannel:AttributesByKeys:Options:completion:)<li>[clearChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearChannel:Options:AttributesWithCompletion:) | 每 5 秒 10 次          |
| 频道属性查询(一并计算）| <li>[getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAllAttributes:completion:) <li>[getChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAttributes:ByKeys:completion:) | 每 5 秒 10 次          |
| 订阅指定单个或多个用户的在线状态   | [subscribePeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/subscribePeersOnlineStatus:completion:) | 每 5 秒 10 次 |
| 退订指定单个或多个用户的在线状态    | [unSubscribePeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/unsubscribePeersOnlineStatus:completion:) | 每 5 秒 10 次 |
| 获取某特定内容被订阅的用户列表   | [queryPeersBySubscriptionOption](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersBySubscriptionOption:completion:) | 每 5 秒 10 次 |
	
<div class="alert note"><sup>1</sup> 加入相同频道的频率限制为每 5 秒 2 次。</div>
	
## 超时设定

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| 功能 | 超时设定 | 
| ---------------- | ---------------- | 
| 登陆 Agora RTM 系统   | 6 秒    | 
| 发送点对点消息  | 10 秒    | 
| 查询用户在线状态  | 10 秒    | 
| 订阅或退订指定用户状态  | 10 秒    | 
| 根据订阅类型获取被订阅用户列表  | 5 秒    | 
| 用户属性或频道属性相关操作  | 5 秒    | 
| 查询单个或多个指定频道成员人数  | 5 秒    | 
| 加入指定频道  | 5 秒    | 
| 发送频道消息 | 5 秒    | 
| 获取当前频道成员列表  | 5 秒    | 



	
## 字符串长度限制

- 点对点或频道消息的字符串最大长度为 32 KB。详见：[AgoraRtmMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) 。
- 呼叫邀请内容的字符串最大长度为 8 KB. 详见：  [AgoraRtmLocalInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) 。
- 呼叫邀请响应的字符串最大长度为 8 KB. 详见： [AgoraRtmRemoteInvitation](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) 。

## 编码格式限制

仅支持发送 UTF-8 编码格式的频道消息和点对点消息、呼叫邀请 content、呼叫邀请 response。

## 其他 


- 当频道人数超过 512 人时，用户进出频道提示会被自动关闭。
- 支持单次查询最多 256 个用户的在线状态。
- 单次用户属性设置的最大值为 16 KB，单次频道属性设置的最大值为 32 KB，单条用户或频道属性的最大值为 8 KB，单次属性操作设置的属性条目不能超过 32 个。
