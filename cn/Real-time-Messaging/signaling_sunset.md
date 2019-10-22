
---
title: 信令维护计划及兼容性说明
description: 
platform: All Platforms
updatedAt: Tue Oct 22 2019 06:03:15 GMT+0800 (CST)
---
# 信令维护计划及兼容性说明
## 信令 SDK 维护计划

- 2019 年 8 月 1 日起： 信令进入维护状态，只进行故障修复，不会增添新功能。信令文档和下载链接将于 2019 年 8 月 12 日被迁移到[这里](https://docs.agora.io/cn/Signaling/product_signaling?platform=All%20Platforms)。
- 2019 年 12 月 31 日起：信令停止维护， 不再修复SDK 故障, 服务端只提供日常最低限度维护以保障信令 SDK 仍然可用。
- 2020 年 6 月 30 日起：信令停止服务，关停信令服务器。


## RTM 与信令互通兼容

Agora RTM SDK v1.0 支持与信令 SDK 互通，但仍不支持以下不兼容功能或版本，我们将在近期通过 [控制台](https://sso.agora.io/cn/login) 站内邮件通知使用了不兼容信令版本或功能的项目，请留意你的 [控制台](https://sso.agora.io/cn/login)。

| 不兼容内容                              | 说明                                                         | 修改建议                                                     |
| -------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------- |
| 使用信令SDK特殊功能          | 使用了如下的特殊功能：<li>特殊频道： <code>\__agora_channel_event</code>、<code>\__agora_user_online</code>  <li>特殊频道属性：  <code>_userNotification</code>、<code> _auto_update_total_num</code> <li>invoke 特殊远程过程调用功能： <code>channel_query_userlist_all</code> <li> 特殊 SDK 配置： <code>setBackground</code> | <li><code>_agora_channel_event</code> 及<code>_agora_user_online</code> 相关功能将于未来支持，届时我们会联系你。<li> 暂无计划支持其他特殊功能。 |
| 信令 SDK 版本号过低            | 使用了版本号低于 v1.1 的信令 SDK     | 升级到最新的信令 SDK 版本，并通知我们在后台开启该项目与 RTM 的互通。 |
| 使用信令 SDK 的小程序版本 |                                                                  | 计划于未来支持。|
| 信令 Web SDK 使用非字符串类型作为消息发送 | 在方法 `messageInstantSend(peer, msg, cb)` 或 `messageChannelSend(msg, cb)` 中的使用非 `String` 类型作为 `msg` 参数 | 修改集成代码，并通知我们在后台开启该项目与 RTM 的互通。 |
| 使用 PSTN 功能                      |                                                                 | 暂无计划支持。|

声网会持续推动兼容性问题的解决，如遇到兼容性相关问题或其他需要请联系支持团队或 support@agora.io。
	
> 如果你在已有的项目中使用了信令，可以参考[升级到 RTM](../../cn/Real-time-Messaging/rtm_signaling_android.md)，帮助你更快速的迁移到 Agora RTM SDK。







