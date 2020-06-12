
---
title: 限制条件
description: 
platform: Linux CPP
updatedAt: Fri Jun 12 2020 02:42:13 GMT+0800 (CST)
---
# 限制条件

本页简要介绍 Agora RTM C++ SDK for Linux 的使用限制条件，包括调用频率、字符串大小、编码格式等。



## 调用频率限制

所有的调用频率都针对单个 <code>IRtmService</code> 实例。如果一个操作对应多个方法，则此操作在单位时间内的调用次数等于所有方法单位时间内的调用次数之和。

<div class="alert note">你可以通过创建多个实例提高 API 的调用频率。</div>

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| 功能                                                  | 方法                                                      | 调用频率上限                |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| 登录到 Agora RTM 系统                              | [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2433a0babbed76ab87084d131227346b) | 每秒 2 次       |
| 查询单个或多个频道的成员人数 | [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a41dee47c6201acb2f29371b6e30249a5) | 每秒 1 次 |
| 加入频道<sup>1</sup> | [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel.html#a6a54cdd8e5db526514e0ca84aa9cba4c) | 每 3 秒 50 次 |
| 发送消息（点对点和频道消息一并计算） | <li>[sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#afec5391fa9c4ec2bfe9ac4e684705600) <li>[sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a08c1b3d444af5a2778ede48e4c677a52)<li>[sendMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel.html#a4ae01f44d49f334f7c2950d95f327d30) | 每秒 60 次          |
| 获取频道成员列表                    | [getMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel.html#a3f9c943059ac48a568c81798da38c3cb) | 每 2 秒 5 次 |
| 更新 token| [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2c33be67bfec02d69041f1e8978f4559) | 每秒 2 次 |
| 查询指定用户在线状态 | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3add0055c4455dc8d04bfc37edfd8e94) | 每 5 秒 10 次 |
| 用户属性增删修改（一并计算）| <li>[setLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a86dcbfc38c665be8565f06c534338d33)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a0a63923bd1e81e60d6ca54213a329747)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#acb669f6c4c28e08cdf889df11e1ddeb3)<li>[clearLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#acc5eee875f4166fe455cde7aff1ad738) | 每 5 秒 10 次          |
| 用户属性查询（一并计算）| <li>[getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a14cac887f9adb390621dd0427092a65b)<li>[getUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#af011235917c291df5581f92afa35532f) | 每 5 秒 40 次          |
| 频道属性增删修改（一并计算）| <li>[setChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#aa229a7207062b510799166c1239412fa)<li>[addOrUpdateChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#ae4068ff21c8e20e8eeb45ba21959c368)<li>[deleteChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a1a448f33be57b31f9952822426e5c4bd)<li>[clearChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#aff6cff676e3fc3150ef5f27845c9a3d3) | 每 5 秒 10 次          |
| 频道属性查询（一并计算）| <li>[getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3dc8409ed82d8f95a0839d5e9e7da564)<li>[getChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#ac97f24f9d78e885e494a22be95db8d33) | 每 5 秒 10 次          |
| 订阅指定单个或多个用户的在线状态   | [subscribePeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3a0e2d4d79ac85e23eae0dcb114ba9f0) | 每 5 秒 10 次 |
| 取消订阅指定单个或多个用户的在线状态    | [unSubscribePeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a027574f04151a9fded678fadba47441e) | 每 5 秒 10 次 |
| 获取某特定内容被订阅的用户列表   | [queryPeersBySubscriptionOption](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a063bd3db39660a7a3513378ce03f4456) | 每 5 秒 10 次 |

<div class="alert note"><sup>1</sup> 加入相同频道的频率限制为每 5 秒 2 次。</div>

## 操作超时时间

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| 操作 | 超时时间 | 
| ---------------- | ---------------- | 
| 登录 Agora RTM 系统   | 6 秒  | 
| 发送点对点消息  | 10 秒    | 
| 查询用户在线状态  | 10 秒    | 
| 订阅或取消订阅指定用户状态  | 10 秒    | 
| 根据订阅类型获取被订阅用户列表  | 5 秒    | 
| 用户属性或频道属性相关操作  | 5 秒    | 
| 查询单个或多个指定频道成员人数  | 5 秒    | 
| 加入指定频道  | 5 秒    | 
| 发送频道消息 | 10 秒    | 
| 获取当前频道成员列表  | 5 秒    | 




## 字符串长度限制

- 点对点或频道消息的字符串最大长度为 32 KB。详见 [IMessage.setText](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_message.html#a2e93098d5a3819e9d4cf8d42641474ae) 。
- 呼叫邀请内容的字符串最大长度为 8 KB. 详见 [ILocalCallInvitation.setContent](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_local_call_invitation.html#aaf4b356b7602cad2bdb998250617c920) 。
- 呼叫邀请响应的字符串最大长度为 8 KB. 详见 [IRemoteCallInvitation.setResponse](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_remote_call_invitation.html#a77c8a0902f9dce36525a66575a6ae8a5) 。


## 编码格式限制

仅支持发送 UTF-8 编码格式的频道消息和点对点消息、呼叫邀请 content、呼叫邀请 response。

## 其他 


- 当频道人数超过 512 人时，用户进出频道提示会被自动关闭。
- 支持单次查询最多 256 个用户的在线状态。
- 对于单个实例，单次最多订阅 512 人的在线状态，累计最多也只能订阅 512 人的在线状态。
- 单次用户属性设置的最大值为 16 KB，单次频道属性设置的最大值为 32 KB，单条用户或频道属性的最大值为 8 KB，单次属性操作设置的属性条目不能超过 32 个。


