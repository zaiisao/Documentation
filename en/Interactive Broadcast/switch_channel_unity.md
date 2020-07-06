
---
title: Switch Channels
description: 
platform: Unity
updatedAt: Mon Jul 06 2020 10:34:56 GMT+0800 (CST)
---
# Switch Channels
## Introduction

If the audience of live interactive video streaming wants to leave for another channel, usually you need to call the following methods:

- The `LeaveChannel` method to leave the current channel.
- The `JoinChannelByKey` method to join the new channel.

Starting with v2.9.0, the Agora Unity SDK provides the `SwitchChannel` method for the audience to quickly switch between live interactive video streaming channels while staying connected to Agora. With this method, you can achieve a much faster channel switch than with the `LeaveChannel` and `JoinChannelByKey` methods. 

## Implementation

Before implementing the quick switch function in your project, ensure that you have implemented the basic live interactive video streaming in your project. For details, see [Start Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_unity.md) or [Start Live Interactive Audio Streaming](../../en/Interactive%20Broadcast/start_live_audio_unity.md).

After the audience joins a live interactive video streaming channel, call the `SwitchChannel` method to enable the audience to switch to another live interactive video streaming channel. Pass in the token and channel name of the new channel in this method.

A successful channel switch with this method triggers the `OnLeaveChannelHandler` and `OnJoinChannelSuccessHandler` callbacks.

### API call sequence

The following diagram shows the API call sequence for channel switching:
![](https://web-cdn.agora.io/docs-files/1585556147266)

### Sample code

You can refer to the following code snippets and implement quick switching in your project:

```C#
mRtcEngine.SwitchChannel(YOUR_TOKEN, mRoomName);
```

### API reference

- [SwitchChannel](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a7f27478a9fe819fc3bdec0164111e2d2)

## Considerations

The `SwitchChannel` method takes effect only when the client role is AUDIENCE.

