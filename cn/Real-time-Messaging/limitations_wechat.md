
---
title: 限制条件
description: 
platform: 微信小程序
updatedAt: Tue Sep 01 2020 03:58:27 GMT+0800 (CST)
---
# 限制条件
本页简要介绍 Agora RTM 微信小程序 SDK （Beta）的使用限制条件，包括调用频率、字符串大小、编码格式等。

## 调用频率限制

所有的调用频率都针对单个 `RtmClient` 实例。如果一个操作对应多个方法，则此操作在单位时间内的调用次数等于所有方法单位时间内的调用次数之和。

<div class="alert note">Agora 不建议在微信小程序平台上通过创建多个实例提高 API 的调用频率。</div>

| 操作                | 方法                             | 调用频率上限   |
| :------------------ | :------------------------------- | :------------- |
| 登录 Agora RTM 系统 | [`login`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_wechat/v1.0.0/classes/rtmclient.html?transId=6cc4d530-d25a-11ea-8288-1373da5192cc#login)                          | 每秒 2 次      |
| 每次加入同一个频道  | [`join`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_wechat/v1.0.0/classes/rtmchannel.html?transId=6cc4d530-d25a-11ea-8288-1373da5192cc#join)                           | 每 3 秒 50 次  |
| 每次加入不同频道    | [`join`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_wechat/v1.0.0/classes/rtmchannel.html?transId=6cc4d530-d25a-11ea-8288-1373da5192cc#join)                           | 每 5 秒 2 次   |
| 发送消息            | <code><a href ="/cn/Real-time-Messaging/API%20Reference/RTM_wechat/v1.0.0/classes/rtmclient.html?transId=6cc4d530-d25a-11ea-8288-1373da5192cc#sendmessagetopeer">sendMessageToPeer</a></code><br><code><a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_wechat/v1.0.0/classes/rtmchannel.html?transId=6cc4d530-d25a-11ea-8288-1373da5192cc#sendmessage">SendMessage</a></code> | 每 3 秒 180 次 |
| 获取频道成员列表    | [`getMembers`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_wechat/v1.0.0/classes/rtmchannel.html?transId=6cc4d530-d25a-11ea-8288-1373da5192cc#getmembers)                     | 每 2 秒 5 次   |
| 更新 Token          | [`renewToken`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_wechat/v1.0.0/classes/rtmclient.html?transId=6cc4d530-d25a-11ea-8288-1373da5192cc#renewtoken)                     | 每秒 2 次      |

## 字符串长度限制

点对点或频道消息的字符串最大长度为 32 KB。详见 [`RtmMessage.text`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_wechat/v1.0.0/interfaces/rtmtextmessage.html?transId=6cc4d530-d25a-11ea-8288-1373da5192cc#text)。

## 编码格式限制

频道消息和点对点消息仅支持 UTF-8 编码格式。

## 其他限制

当频道人数超过 512 人时，用户进出频道的提示会被自动关闭。
