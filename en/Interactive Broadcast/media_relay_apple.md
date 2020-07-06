
---
title: Co-host across Channels
description: 
platform: iOS,macOS
updatedAt: Mon Jul 06 2020 10:33:35 GMT+0800 (CST)
---
# Co-host across Channels
## Introduction

Co-hosting across channels refers to the process where the Agora SDK relays the media stream of a host from a live interactive streaming channel (source channel) to a maximum of four live interactive streaming channels (destination channels). It has the following benefits:

- All hosts in the relay channels can see and hear each other.
- The audiences in the relay channels can see and hear all hosts.

Co-hosting across channels applies to scenarios such as an online singing contest, where hosts of different channels interact with each other.

## Implementation

<div class="alert note">To enable channel media relay, contact <a href="mailto:support@agora.io">support@agora.io</a>.</div>

Before relaying media streams across channels, ensure that you have implemented the basic real-time communication functions in your project. For details, see the following documents:
- iOS: [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_ios.md).
- macOS: [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_mac.md).

As of v2.9.0, the Agora Native SDK supports co-hosting across channels with the following methods:

- startChannelMediaRelay
- updateChannelMediaRelay
- stopChannelMediaRelay

Once the channel media relay starts, the SDK relays the media streams of a host from the source channel to at most four destination channels.

During the relay, the SDK reports the states and events of the channel media relay with the `channelMediaRelayStateDidChange` and `didReceiveChannelMediaRelayEvent` callbacks. Refer to the following codes and their corresponding states to implement you code logic:

| State codes | Event codes | The media stream relay state |
| ---------------- | ---------------- | ---------------- |
| AgoraChannelMediaRelayStateRunning(2) and AgoraChannelMediaRelayErrorNone(0)      | AgoraChannelMediaRelayEventSentToDestinationChannel(4)     | The channel media relay starts.      |
| AgoraChannelMediaRelayStateFailure(3)      | /     | Exceptions occur for the media stream relay. Refer to the error parameter for troubleshooting.      |
| AgoraChannelMediaRelayStateIdle(0) and AgoraChannelMediaRelayErrorNone(0)      | /     | The channel media relay stops.      |

**Note**:
- Any host in a live interactive streaming channel can call the `startChannelMediaRelay` method to enable channel media stream relay. The SDK relays the media streams of the host who calls the method.
- During the media stream relay, if the host of the destination channel drops offline or leaves the channel, the host of the source channel receives the `didOfflineOfUid` callback.

### API call sequence

Follow the API call sequence to implement your code logic:

![](https://web-cdn.agora.io/docs-files/1568964082213)

### Sample code

```swift
func getMediaRelayConfiguration() -> AgoraChannelMediaRelayConfiguration? {
	guard let list = destinationList else {
		alert(string: "destination list nil")
		return nil
	}
	
	let config = AgoraChannelMediaRelayConfiguration()
	
	if let uid = sourceUid {
		config.sourceInfo.uid = uid
	} else {
		config.sourceInfo.uid = currentUid ?? 0
	}
	
	if let token = sourceToken {
		config.sourceInfo.token = token
	}
	
	for item in list where item.isPrepareForMediaRelay() {
		let info = getRelayInfoFromDestination(item)
		config.setDestinationInfo(info, forChannelName: item.channel)
	}
	
	return config
}


if let config = getMediaRelayConfiguration() {
	// Sets the destination channel you want to join.
	let value = rtcEngine.startChannelMediaRelay(config)
}

if let config = getMediaRelayConfiguration() {
	// Updates the media relay channel.
	let value = rtcEngine.updateChannelMediaRelay(config)
}
```

<div class="alert note">After calling the <code>startChannelMediaRelay</code> method, you can call the <code>updateChannelMediaRelay</code> method to add or delete the relay channels.</div>

We provide an open-source [Cross-Channel-OpenLive-iOS](https://github.com/AgoraIO/Advanced-Video/tree/dev/backup/Cross-Channel/Cross-Channel-OpenLive-iOS) demo project that implements channel media relay on GitHub. You can try the demo and view the source code.

### API Reference

- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)
- [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:)
- [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:)

## Considerations

- The Agora RTC SDK supports relaying media streams to a maximum of four destination channels. To add or delete a destination channel, call `updateChannelMediaRelay`.
- This feature supports integer user IDs only.




- When setting the souce channel information (`setSrcChannelInfo`), ensure that you set `uid` as 0, and the `uid` that you use to generate the token should also be set as 0.

- To call `startChannelMediaRelay` again after it succeeds, you must call `stopChannelMediaRelay` to quit the current relay.
