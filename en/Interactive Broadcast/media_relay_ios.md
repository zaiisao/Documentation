
---
title: Co-host across Channels
description: 
platform: iOS
updatedAt: Mon Oct 21 2019 06:01:55 GMT+0800 (CST)
---
# Co-host across Channels
## Introduction

By co-hosting across channels, the Agora SDK relays the media streams of a host from one Live Broadcast channel (source channel) to other Live Broadcast channels (destination channels), for the following purposes:

- All hosts in the relay channels can see and hear each other.
- All the audience in the relay channels can see and hear all the hosts.

Co-hosting across channels applies to scenarios such as online singing contests, where hosts of different channels interact with each other.

## Implementation

Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md).

The Agora Native SDK and later support co-hosting across channels with the following methods:

- startChannelMediaRelay
- updateChannelMediaRelay
- stopChannelMediaRelay

Once the channel media relay starts, the SDK relays the media streams of a host from the source channel to at most four destination channels.

During the relay, the SDK reports the states and events of the channel media relay with the `channelMediaRelayStateDidChange` and `didReceiveChannelMediaRelayEvent` callbacks. Refer to the following codes and their corresponding states to implement you code logic:

| State codes | Event codes | The media stream relay state |
| ---------------- | ---------------- | ---------------- |
| AgoraChannelMediaRelayStateRunning(2) and AgoraChannelMediaRelayErrorNone(0)      | AgoraChannelMediaRelayEventSentTo-DestinationChannel(4)     | The channel media relay starts.      |
| AgoraChannelMediaRelayStateFailure(3)      | /     | Exceptions occur for the media stream relay. Refer to the error parameter for troubleshooting.      |
| AgoraChannelMediaRelayStateIdle(0) and AgoraChannelMediaRelayErrorNone(0)      | /     | The channel media relay stops.      |

**Note**:
- Any host in a Live Broadcast channel can call the `startChannelMediaRelay` method to enable channel media stream relay. The SDK relays the media streams of the host who calls the method.
- During the media stream relay, if the host of the destination channel drops offline or leaves the channel, the host of the source channel receives the `didOfflineOfUid` callback.

### API call sequence

Follow the API call sequence to implent your code logic:

![](https://web-cdn.agora.io/docs-files/1565776086454)

### Code snippets

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

**Note**:
After calling the `startChannelMediaRelay` method, you can call the `updateChannelMediaRelay` method to add or delete the relay channels.

### API Reference

- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.9.0/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)
- [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:)
- [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.9.0/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:)

## Considerations

- Co-hosting across channels supports relaying media streams to at most four destination channels. During the relay, you can call the `updateChannelMediaRelay` method to add or delete the destination channels.
- Channel media stream relay does not support string user accounts.
- After a successful call of the `startChannelMediaRelay` method, if you want to call this method again, ensure that you call the `stopChannelMediaRelay` method.
