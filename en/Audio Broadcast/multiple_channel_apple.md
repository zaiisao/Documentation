
---
title: Join Multiple Channels
description: 3.0 feature multi-channel
platform: iOS,macOS
updatedAt: Thu Jun 04 2020 08:48:12 GMT+0800 (CST)
---
# Join Multiple Channels
## Introduction

As of v3.0.0, the Agora RTC SDK enables users to join an unlimited number of channels at a time and to receive the audio and video streams of all the channels.

## Implementation

The RTC SDK uses the `AgoraRtcChannel` and `AgoraRtcChannelDelegate` classes to support the multi-channel function. To implement this function in your project, choose either of the following approaches:

### Approach 1: Use the AgoraRtcChannel class only

Follow these steps to implement the multi-channel function in your project:

1. Call `createRtcChannel` of the `AgoraRtcEngineKit` class to create an `AgoraRtcChannel` object with a channel ID.
2. Call `joinChannelByToken` of the `AgoraRtcChannel` class to join the created channel.
3. Repeat steps 1 and 2 to create multiple `AgoraRtcChannel` objects, and call the `joinChannelByToken` method of each object to join these channels.

<div class="alert note">
	<li>Once you have joined multiple channels, call the <code>publish</code> method of a specific <code>AgoraRtcChannel</code> object to publish the stream to the corresponding channel. Note that you can publish one stream in only one channel at a time.
	<li>After calling the <code>publish</code> method in Channel 1, you need to call the <code>unpublish</code> method in Channel 1 before calling the <code>publish</code> method in Channel 2.
</div>

This approach applies to scenarios where you need to frequently switch between channels and publish audio or video streams.

**API call sequence**

Refer to the following API sequence to implement the multi-channel function in your project with the `AgoraRtcChannel` class.

![](https://web-cdn.agora.io/docs-files/1575881443248)

**Sample code**

You can alse refer to the following sample code.

```Objective-C
- (BOOL)JoinChannelWithAgoraRtcChannel:(NSString *)channelId token:(NSString *)token info:(NSString *)info uid:(NSUInteger)uid {
    
    if (channelId.length <= 0) return NO;

    // 1. Create agoraRtcEngineKit
    AgoraRtcEngineKit *kit = [AgoraRtcEngineKit sharedEngineWithAppId:@"AppId" delegate:self];
    
    // 2. Create agoraRtcChannel
    AgoraRtcChannel *agoraRtcChannel = [kit createRtcChannel:channelId];
    if (agoraRtcChannel == nil) return NO;
    
    // 3. Set rtcChannelDelegate
    [agoraRtcChannel setRtcChannelDelegate:self];
    
    // 4. Configurate mediaOptions
    AgoraRtcChannelMediaOptions *mediaOptions = [AgoraRtcChannelMediaOptions new];
    mediaOptions.autoSubscribeAudio = YES;
    mediaOptions.autoSubscribeVideo = YES;
    
    // 5. Join channel
    [agoraRtcChannel joinChannelByToken:token info:info uid:uid options:mediaOptions];
    
  	return YES;
}
```

### Approach 2: Use both the AgoraRtcEngineKit and AgoraRtcChannel classes

Follow these steps to implement the multi-channel function in your project:

1. Call `joinChannelByToken` of the `AgoraRtcEngineKit` class to join the first channel.
2. Call `createRtcChannel` of the `AgoraRtcEngineKit` class to create an `AgoraRtcChannel` object with a channel ID.
3. Call `joinChannelByToken` of the `AgoraRtcChannel` class to join the second channel.
4. Repeat steps 2 and 3 to create multiple `AgoraRtcChannel` objects, and call the `joinChannelByToken` method of each object to join these channels.

This approach applies to scenarios where you publish streams only to a specified channel. If you have implemented RTC functions with an earlier version of our SDK, this approach does not require any change to your existing code.

**API call sequence**

Refer to the followng API sequence to implement the multi-channel function in your project with the `AgoraRtcEngineKit` and `AgoraRtcChannel` classes.

![](https://web-cdn.agora.io/docs-files/1575881689780)

We also provide an open-source sample project [Breakout Class](https://github.com/AgoraIO-Usecase/Breakout-Class/tree/master/breakout-ios) on GitHub that implements the multi-channel function. You can download it to better understand the code logic.

### API reference

- [`createRtcChannel`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/v3.0.0/Classes/AgoraRtcEngineKit.html#//api/name/createRtcChannel::)
- [`AgoraRtcChannel`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/v3.0.0/Classes/AgoraRtcChannel.html) 
- [`AgoraRtcChannelDelegate`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/v3.0.0/Protocols/AgoraRtcChannelDelegate.html)

## Considerations

### Setting the remote video

In video scenarios, if the remote user joins the channel using `AgoraRtcChannel`, ensure that you specify the channel ID of the remote user in  [AgoraRtcVideoCanvas](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/v3.0.0/Classes/AgoraRtcVideoCanvas.html) when setting the remote video view. 

### Limits on publishing the stream

v3.0 supports publishing one stream to only one channel at a time. In other words, if you are already publishing a stream to a channel, you cannot publish that stream to a different channel.

You must meet the following requirements to implementing the multi-channel function. Otherwise, the  SDK returns `AgoraErrorCodeRefused(-5)`:

- When joining multiple channels using the `joinChannelByToken` method in the `AgoraRtcChannel` class:
  - After calling the `publish` method in Channel 1, you need to call the `unpublish` method in Channel 1 before calling the publish method in Channel 2.
- When joining multiple channels using both the `AgoraRtcEngineKit` and `AgoraRtcChannel` classes:
  - In the Communication profile, if you join Channel 1 using the `AgoraRtcEngineKit` class, and Channel 2 using the `AgoraRtcChannel` class, you cannot call `publish` in the `AgoraRtcChannel` class.
  - In the Live-Broadcast profile, if you join Channel 1 as a broadcaster using the `AgoraRtcEgineKit` class, and Channel 2 through the `AgoraRtcChannel` class, you cannot call `publish` in the `AgoraRtcChannel` class.
  - In the Live-Broadcast profile, after joining multiple channels, you cannot call the `publish` method in the `AgoraRtcChannel` class as an AUDIENCE.
  - After calling `publish` in the `AgoraRtcChannel` class, you need to call `unpublish` in the `AgoraRtcChannel` class. Otherwise, you cannot call `joinChannelByToken` in the `AgoraRtcEngineKit` class.
