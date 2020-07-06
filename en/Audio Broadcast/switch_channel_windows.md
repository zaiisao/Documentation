
---
title: Switch Channels
description: 
platform: Windows
updatedAt: Mon Jul 06 2020 10:32:50 GMT+0800 (CST)
---
# Switch Channels
## Introduction

If the audience of live interactive video streaming wants to leave for another channel, usually you need to call the following methods:

- The `leaveChannel` method to leave the current channel.
- The `joinChannel` method to join the new channel.

Starting with v2.9.0, the Agora Native SDK provides the `switchChannel` method for the audience to quickly switch between live interactive video streaming channels while staying connected to Agora. With this method, you can achieve a much faster channel switch than with the `leaveChannel` and `joinChannel` methods. 

## Implementation

Before implementing the quick switch function in your project, ensure that you have implemented the basic live interactive video streaming in your project. For details, see [Start Live Interactive Video Streaming](../../en/Audio%20Broadcast/start_live_windows.md).

After the audience joins a live interactive video streaming channel, call the `switchChannel` method to enable the audience to switch to another live interactive video streaming channel. Pass in the token and channel name of the new channel in this method.

A successful channel switch with this method triggers the `onLeaveChannel` and `onJoinChannelSuccess` callbacks.

### API call sequence

The following diagram shows the API call sequence for channel switching:

![](https://web-cdn.agora.io/docs-files/1569229455514)

### Sample code

You can refer to the following code snippets and implement quick switching in your project:

```C++
m_lpAgoraEngine->switchChannelChannel(YOUR_TOKEN, channelName)
```

### API reference

- [switchChannel](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab)

## Considerations

The `switchChannel` method takes effect only when the client role is AUDIENCE.
