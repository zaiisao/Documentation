
---
title: 如何实现业务层的通话计费？
description: 
platform: All Platforms
updatedAt: Wed Mar 25 2020 12:27:04 GMT+0800 (CST)
---
# 如何实现业务层的通话计费？
为满足不同业务场景的计费需求，Agora 建议业务层将用户的通话时长作为计费的标准之一。你可以结合 Agora RTC SDK 计算用户的通话时长，或使用 Agora RTM SDK 检测消息服务和客户端的连接状态。

## 实现方法

- 正常情况下，用户成功加入 RTC 频道后会触发加入频道回调（如 `onJoinChannelSuccess`），成功离开 RTC 频道后会触发离开频道回调（如 `onLeaveChannel`）。
  - 你可以将触发加入频道回调的时刻记为通话开始，将触发离开频道回调的时刻记为通话结束，再结合你的业务逻辑，计算出用户的通话时长并计费。
  - 在 Native SDK 中，你还可以通过离开频道回调中报告的 `duration` 参数直接获取用户的通话时长，并实现计费。

- 异常情况下，例如断线，你可以在 [Agora RTM SDK](https://docs.agora.io/cn/Real-time-Messaging/reconnecting_android?platform=Android) 或其他信令系统中开启心跳机制检测，检测消息服务和客户端的连接状态。根据连接状态，你可以结合自己的业务逻辑统计该通话的时长，并实现计费。

## 各语言方法对照表

本文提及的方法名均为 Java 语言。其他语言对应的方法名如下表所示：

| 描述         | Java/C++             | OC                       | Javascript                                                   |
| :----------- | :------------------- | :----------------------- | :----------------------------------------------------------- |
| 加入频道回调 | `onJoinChannelSuccess` | `didJoinChannel`           | `Client.on("connected")` 或 `Client.on("connection-state-change")` |
| 离开频道回调 | `onLeaveChannel`       | `didLeaveChannelWithStats` | `Client.on("connection-state-change")`                         |
