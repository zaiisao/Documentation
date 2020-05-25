
---
title: 限制条件
description: RTM Web Limitations
platform: Web
updatedAt: Mon May 25 2020 06:49:03 GMT+0800 (CST)
---
# 限制条件

本页简要介绍 Agora RTM Web SDK 的使用限制条件，包括调用频率、字符串大小、编码格式等。


## 调用频率限制

所有的调用频率都针对单个 <code>RtmClient</code> 实例。如果一个操作对应多个方法，则此操作在单位时间内的调用次数等于所有方法单位时间内的调用次数之和。

<div class="alert note">我们<b>暂不建议</b>在 Web 平台上通过创建多个实例提高 API 的调用频率。</div>

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| 功能                                                  | 方法                                                      | 调用频率上限                |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| 登录到 Agora RTM 系统                              | [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login) | 每秒 2 次         |
| 查询单个或多个频道的成员人数 | [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelmembercount) | 每秒 1 次 |
| 每次加入同一个频道<sup>1</sup> | [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join) | 每 3 秒 50 次 |
| 每次加入不同频道<sup>1</sup> | [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join) | 每 5 秒 2 次 |
| 发送消息（点对点和频道消息一并计算在内） | <li>[sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#sendmessagetopeer) <li>[SendMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#sendmessage) | 每秒 60 次          |
| 获取频道成员列表                    | [getMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#getmembers) | 每 2 秒 5 次 |
| 更新 Token                               | [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#renewtoken) | 每秒 2 次         |
| 查询指定用户在线状态                               | [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersonlinestatus) | 每 5 秒 10 次        |
| 用户属性增删修改（一并计算）| <li>[setLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setlocaluserattributes)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatelocaluserattributes)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletelocaluserattributesbykeys)<li>[clearLocalUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearlocaluserattributes) | 每 5 秒 10 次          |
| 用户属性查询（一并计算）| <li>[getUserAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)<li>[getUserAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys) | 每 5 秒 40 次          |
| 频道属性增删修改（一并计算）| <li>[setChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setchannelattributes)<li>[addOrUpdateChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatechannelattributes)<li>[deleteChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletechannelattributesbykeys)<li>[clearAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearchannelattributes) | 每 5 秒 10 次          |
| 频道属性查询（一并计算）| <li>[getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributes)<li>[getChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributesbykeys) | 每 5 秒 10 次          |
| 订阅指定单个或多个用户的在线状态   | [subscribePeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#subscribepeersonlinestatus) | 每 5 秒 10 次 |
| 取消订阅指定单个或多个用户的在线状态    | [unSubscribePeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#unsubscribepeersonlinestatus) | 每 5 秒 10 次 |
| 获取某特定内容被订阅的用户列表   | [queryPeersBySubscriptionOption](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersbysubscriptionoption) | 每 5 秒 10 次 |

	
## 操作超时时间

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| 操作 | 超时时间 | 
| ---------------- | ---------------- | 
| 登录 Agora RTM 系统   | 6 秒<sup>I</sup>  | 
| 发送点对点消息  | 10 秒<sup>II</sup>    | 
| 查询用户在线状态  | 10 秒    | 
| 订阅或取消订阅指定用户状态  | 10 秒    | 
| 根据订阅类型获取被订阅用户列表  | 5 秒    | 
| 用户属性或频道属性相关操作  | 5 秒    | 
| 查询单个或多个指定频道成员人数  | 5 秒    | 
| 加入指定频道  | 5 秒    | 
| 发送频道消息 | 10 秒    | 
| 获取当前频道成员列表  | 5 秒    | 


 
<div class="alert note">自 v1.2.2 起: <p>I. Web 平台的登录超时由 6 秒改为 10 秒。<p>II. Web 平台的点对点消息发送超时时间由 5 秒更新为 10 秒。</div>


## 字符串长度限制

- 点对点或频道消息的字符串最大长度为 32 KB。详见 [RtmMessage.text](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmtextmessage.html#text) 。
- 呼叫邀请内容的字符串最大长度为 8 KB。详见 [LocalInvitation.content](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#content) 。
- 呼叫邀请响应的字符串最大长度为 8 KB。详见 [RemoteInvitation.response](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#response) 。

## 编码格式限制

仅支持发送 UTF-8 编码格式的频道消息和点对点消息。
	
## 浏览器兼容性
	
详见[开发环境要求](https://docs.agora.io/cn/Real-time-Messaging/messaging_web?platform=Web#%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83%E8%A6%81%E6%B1%82)。


## 编码格式限制

仅支持发送 UTF-8 编码格式的频道消息和点对点消息、呼叫邀请 content、呼叫邀请 response。

## 其他 


- 当频道人数超过 512 人时，用户进出频道提示会被自动关闭。
- 支持单次查询最多 256 个用户的在线状态。
- 对于单个实例，单次最多订阅 512 人的在线状态，累计最多也只能订阅 512 人的在线状态。
- 单次用户属性设置的最大值为 16 KB，单次频道属性设置的最大值为 32 KB，单条用户或频道属性的最大值为 8 KB，单次属性操作设置的属性条目不能超过 32 个。
