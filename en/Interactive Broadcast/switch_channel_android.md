
---
title: Switch Channels
description: 
platform: Android
updatedAt: Mon Jul 06 2020 10:26:17 GMT+0800 (CST)
---
# Switch Channels
## Introduction

If the audience of live interactive video streaming wants to leave for another channel, usually you need to call the following methods:

- The `leaveChannel` method to leave the current channel.
- The `joinChannel` method to join the new channel.

Starting with v2.9.0, the Agora Native SDK provides the `switchChannel` method for the audience to quickly switch between live interactive video streaming channels while staying connected to Agora. With this method, you can achieve a much faster channel switch than with the `leaveChannel` and `joinChannel` methods. 

## Implementation

Before implementing the quick switch function in your project, ensure that you have implemented the basic live interactive video streaming in your project. For details, see [Start Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_android.md).

After the audience joins a live interactive video streaming channel, call the `switchChannel` method to enable the audience to switch to another live interactive video streaming channel. Pass in the token and channel name of the new channel in this method.

A successful channel switch with this method triggers the `onLeaveChannel` and `onJoinChannelSuccess` callbacks.

### API call sequence

The following diagram shows the API call sequence for channel switching:

![](https://web-cdn.agora.io/docs-files/1569229438599)

### Sample code

You can refer to the following code snippets and implement quick switching in your project:

```java
mRtcEngine.switchChannel(YOUR_TOKEN, mRoomName);
```

We also provide an open-source demo project that implements [Quick-Switch-Android](https://github.com/AgoraIO/Advanced-Video/tree/dev/backup/Quick-Switch-Channel/Quick-Switch-Android) on GitHub. You can try the demo and view the source code.

### API reference

- [switchChannel](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a72f13225defc1b14dfb29820a0495da2)

## Considerations

The `switchChannel` method takes effect only when the client role is AUDIENCE.
