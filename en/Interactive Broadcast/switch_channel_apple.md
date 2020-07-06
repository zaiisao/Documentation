
---
title: Switch Channels
description: 
platform: iOS,macOS
updatedAt: Mon Jul 06 2020 10:29:35 GMT+0800 (CST)
---
# Switch Channels
## Introduction

If the audience of live interactive video streaming wants to leave for another channel, usually you need to call the following methods:

- The `leaveChannel` method to leave the current channel.
- The `joinChannelByToken` method to join the new channel.

Starting with v2.9.0, the Agora Native SDK provides the `switchChannel` method for the audience to quickly switch between live interactive video streaming channels while staying connected to Agora. With this method, you can achieve a much faster channel switch than with the `leaveChannel` and `joinChannelByToken` methods. 

## Implementation

Before implementing the quick switch function in your project, ensure that you have implemented the basic live interactive video streaming in your project. For details, see the following documents:
- iOS: [Start Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_ios.md)
- macOS: [Start Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_mac.md)

After the audience joins a live interactive video streaming channel, call the `switchChannelByToken` method to enable the audience to switch to another live interactive video streaming channel. Pass in the token and channel name of the new channel in this method.

A successful channel switch with this method triggers the `didLeaveChannel` and `didJoinChannel` callbacks.

### API call sequence

The following diagram shows the API call sequence for channel switching:

![](https://web-cdn.agora.io/docs-files/1569229120252)

### Sample code

You can refer to the following code snippets and implement quick switching in your project:

```swift
agoraKit.switchChannel(byToken: nil, channelId: channel.channelName, joinSuccess: nil)
```

We also provide an open-source demo project that implements [Quick-Switch-iOS](https://github.com/AgoraIO/Advanced-Video/tree/dev/backup/Quick-Switch-Channel/Quick-Switch-iOS) on GitHub. You can try the demo and view the source code.

### API reference

- [switchChannelByToken](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:)

## Considerations

The `switchChannelByToken` method takes effect only when the client role is AUDIENCE.
