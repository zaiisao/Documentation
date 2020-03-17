
---
title: Agora RTM SDK 支持从频道外向频道内发送消息吗？
description: 
platform: All Platforms
updatedAt: Fri Feb 28 2020 20:59:32 GMT+0800 (CST)
---
# Agora RTM SDK 支持从频道外向频道内发送消息吗？
Agora RTM SDK 暂不支持从频道外向频道内发送消息。你必须加入频道才能向该频道成员发送频道消息。不过，你也可以通过设置频道属性并在每次 API 调用时开启 enableNotificationToChannelMembers 通知该频道所有成员本次更新。
