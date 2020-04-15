
---
title: How can I bill individuals?
description: 
platform: All Platforms
updatedAt: Thu Mar 26 2020 11:07:38 GMT+0800 (CST)
---
# How can I bill individuals?
Agora recommends using call duration as one way to bill your users. You can use Agora RTC SDK to calculate call duration, and Agora RTM SDK to check the connection status of the messaging service and the Agora RTC SDK.

## Implementation

- By design, the SDK triggers `onJoinChannelSuccess` when a user successfully joins a channel, and triggers `onLeaveChannel` when a user successfully leaves the channel.
  - Set the time when the SDK triggers `onJoinChannelSuccess` as a start and the time when the SDK triggers `onLeaveChannel `as an end to calculate the call duration, and implement business billing.
  - If you are using Agora RTC Native SDK, you can use the `duration` parameter that `onLeaveChannel` reports to get the call duration for billing purposes.
- Under abnormal conditions, such as poor network connections, can adversely affect an otherwise straightforward calculation of call duration for billing purposes. Use [Agora RTM SDK](https://docs.agora.io/en/Real-time-Messaging/reconnecting_android?platform=Android#connection_state_connected) or a different signaling system to check the connection status of the messaging service and Agora RTC SDK. Combine the connection status and your business scenarios, calculate the call duration for billing purposes.

## Equivalent callbacks in other programming languages 

The callbacks referenced in this article are in Java. Refer to the following table if you are programming in a different language.

| Java/C++             | OC                       | Javascript                                                   |
| :------------------- | :----------------------- | :----------------------------------------------------------- |
| `onJoinChannelSuccess` | `didJoinChannel`           | `Client.on("connected")` or `Client.on("connection-state-change")` |
| `onLeaveChannel`       | `didLeaveChannelWithStats` | `Client.on("connection-state-change")`                         |
