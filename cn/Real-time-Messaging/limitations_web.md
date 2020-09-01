
---
title: 限制条件
description: RTM Web Limitations
platform: Web
updatedAt: Tue Sep 01 2020 06:59:57 GMT+0800 (CST)
---
# 限制条件

本页简要介绍 Agora RTM Web SDK 的使用限制条件，包括调用频率、字符串大小、编码格式等。


## 调用频率限制

所有的调用频率都针对单个 <code>RtmClient</code> 实例。如果一个操作对应多个方法，则此操作在单位时间内的调用次数等于所有方法单位时间内的调用次数之和。

<div class="alert note">我们<b>暂不建议</b>在 Web 平台上通过创建多个实例提高 API 的调用频率。</div>

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| 操作                                                 | 方法                                                      | 调用频率上限                |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| 登录到 Agora RTM 系统                              | [`login`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login) | 每秒 2 次         |
| 查询单个或多个频道的成员人数 | [`getChannelMemberCount`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelmembercount) | 每秒 1 次 |
| 每次加入同一个频道 | [`join`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join) | 每 3 秒 50 次 |
| 每次加入不同频道 | [`join`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join) | 每 5 秒 2 次 |
| 发送消息 | <li>[`sendMessageToPeer`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#sendmessagetopeer) <li>[`SendMessage`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#sendmessage) | 每 3 秒 180 次          |
| 获取频道成员列表                    | [`getMembers`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#getmembers) | 每 2 秒 5 次 |
| 更新 Token                               | [`renewToken`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#renewtoken) | 每秒 2 次         |
| 查询指定用户在线状态                               | [`queryPeersOnlineStatus`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersonlinestatus) | 每 5 秒 10 次        |
| 用户属性增删修改| <li>[`setLocalUserAttributes`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setlocaluserattributes)<li>[`addOrUpdateLocalUserAttributes`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatelocaluserattributes)<li>[`deleteLocalUserAttributesByKeys`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletelocaluserattributesbykeys)<li>[`clearLocalUserAttributes`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearlocaluserattributes) | 每 5 秒 10 次          |
| 用户属性查询| <li>[`getUserAttributes`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)<li>[`getUserAttributesByKeys`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys) | 每 5 秒 40 次          |
| 频道属性增删修改| <li>[`setChannelAttributes`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setchannelattributes)<li>[`addOrUpdateChannelAttributes`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatechannelattributes)<li>[`deleteChannelAttributesByKeys`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletechannelattributesbykeys)<li>[`clearAttributes`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearchannelattributes) | 每 5 秒 10 次          |
| 频道属性查询| <li>[`getChannelAttributes`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributes)<li>[`getChannelAttributesByKeys`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributesbykeys) | 每 5 秒 10 次          |
| 订阅指定单个或多个用户的在线状态   | [`subscribePeersOnlineStatus`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#subscribepeersonlinestatus) | 每 5 秒 10 次 |
| 取消订阅指定单个或多个用户的在线状态    | [`unSubscribePeersOnlineStatus`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#unsubscribepeersonlinestatus) | 每 5 秒 10 次 |
| 根据订阅内容获取用户列表   | [`queryPeersBySubscriptionOption`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersbysubscriptionoption) | 每 5 秒 10 次 |


## 字符串长度限制

- 点对点或频道消息的字符串最大长度为 32 KB。详见 [`RtmMessage.text`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmtextmessage.html#text)。
- 呼叫邀请内容的字符串最大长度为 8 KB。详见 [`LocalInvitation.content`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#content)。
- 呼叫邀请响应的字符串最大长度为 8 KB。详见 [`RemoteInvitation.response`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#response)。

## 编码格式限制

仅支持发送 UTF-8 编码格式的频道消息和点对点消息、呼叫邀请内容、呼叫邀请响应。
	
## 浏览器兼容性
	
详见[开发环境要求](https://docs.agora.io/cn/Real-time-Messaging/messaging_web?platform=Web#%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83%E8%A6%81%E6%B1%82)。


## 其他限制

- 当频道人数超过 512 人时，用户进出频道的提示会被自动关闭。
- 对于单个实例，支持单次查询最多 256 个用户的在线状态。
- 对于单个实例，单次最多且总计只能订阅 512 人的在线状态。你无法通过多次订阅来突破人数限制。如果频道人数超过 512，SDK 会随机返回其中的 512 人。
- 对于单个实例，每个用户最多同时加入 20 个频道。
- 单次用户属性设置的最大值为 16 KB，单次频道属性设置的最大值为 32 KB，单条用户或频道属性（键值对）的最大值为 8 KB，单次属性操作设置的属性条目（键值对）不能超过 32 个。
- 每个上传文件或图片在 Agora 服务器上的保存时间最长为 7 天，上传文件或图片所对应的 media ID 有效期也为 7 天。
- 每个上传文件或图片不得超过 30 MB。
- 对于单个实例，同时最多支持 9 个上传和下载进程。
