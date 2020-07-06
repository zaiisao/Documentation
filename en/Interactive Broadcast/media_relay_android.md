
---
title: Co-host across Channels
description: 
platform: Android
updatedAt: Mon Jul 06 2020 10:30:15 GMT+0800 (CST)
---
# Co-host across Channels
## Introduction

Co-hosting across channels refers to the process where the Agora SDK relays the media stream of a host from a live interactive streaming channel (source channel) to a maximum of four live interactive streaming channels (destination channels). It has the following benefits:

- All hosts in the relay channels can see and hear each other.
- The audiences in the relay channels can see and hear all hosts.

Co-hosting across channels applies to scenarios such as an online singing contest, where hosts of different channels interact with each other.

## Implementation

<div class="alert note">To enable channel media relay, contact <a href="mailto:support@agora.io">support@agora.io</a>.</div>

Before relaying media streams across channels, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_android.md).

As of v2.9.0, the Agora Native SDK supports co-hosting across channels with the following methods:

- startChannelMediaRelay
- updateChannelMediaRelay
- stopChannelMediaRelay

Once the channel media relay starts, the SDK relays the media streams of a host from the source channel to at most four destination channels.

During the relay, the SDK reports the states and events of the channel media relay with the `onChannelMediaRelayStateChanged` and `onChannelMediaRelayEvent` callbacks. Refer to the following codes and their corresponding states to implement your code logic:

| State codes | Event codes | The media stream relay state |
| ---------------- | ---------------- | ---------------- |
| RELAY_STATE_RUNNING(2) and RELAY_OK(0)      | RELAY_EVENT_PACKET_SENT_TO_DEST_CHANNEL(4)     | The channel media relay starts.      |
| RELAY_STATE_FAILURE(3)      | /     | Exceptions occur for the media stream relay. Refer to the error parameter for troubleshooting.      |
| RELAY_STATE_IDLE(0) and RELAY_OK(0)      | /     | The channel media relay stops.      |

**Note**:
- Any host in a live interactive streaming channel can call the `startChannelMediaRelay` method to enable channel media stream relay. The SDK relays the media streams of the host who calls the method.
- During the media stream relay, if the host of the destination channel drops offline or leaves the channel, the host of the source channel receives the `onUserOffline` callback.

### API call sequence

Follow the API call sequence to implement your code logic:

![](https://web-cdn.agora.io/docs-files/1568962085119)

### Sample code

- Starts the channel media relay:

	```java
	ChannelMediaInfo srcChannelInfo = new ChannelMediaInfo(srcChannelName,srcToken,workerSrcUid);
	ChannelMediaRelayConfiguration mediaRelayConfiguration = new ChannelMediaRelayConfiguration();
	// Sets the source channel information.
	mediaRelayConfiguration.setSrcChannelInfo(srcChannelInfo);
	ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName1, destToken1, workerDestUid1);
	mediaRelayConfiguration.setDestChannelInfo(destChannelInfo.channelName,destChannelInfo);
	ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName2, destToken2, workerDestUid2);
	mediaRelayConfiguration.setDestChannelInfo(destChannelInfo.channelName,destChannelInfo);
	ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName3, destToken3, workerDestUid3);
	mediaRelayConfiguration.setDestChannelInfo(destChannelInfo.channelName,destChannelInfo);
	ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName4, destToken4, workerDestUid4);
	// Sets the destination channel information.
	mediaRelayConfiguration.setDestChannelInfo(destChannelInfo.channelName,destChannelInfo);
	int result = worker().getRtcEngine().startChannelMediaRelay(mediaRelayConfiguration);
	```

- Adds or deletes the media relay channels:

	```java
	ChannelMediaInfo srcChannelInfo = new ChannelMediaInfo(srcChannelName,srcToken,workerSrcUid);
	ChannelMediaRelayConfiguration mediaRelayConfiguration = new ChannelMediaRelayConfiguration();
	// Sets the source channel information.
	mediaRelayConfiguration.setSrcChannelInfo(srcChannelInfo);
	ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName1, destToken1, workerDestUid1);
	mediaRelayConfiguration.setDestChannelInfo(destChannelInfo.channelName,destChannelInfo);
	ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName2, destToken2, workerDestUid2);
	mediaRelayConfiguration.setDestChannelInfo(destChannelInfo.channelName,destChannelInfo);
	ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName3, destToken3, workerDestUid3);
	mediaRelayConfiguration.setDestChannelInfo(destChannelInfo.channelName,destChannelInfo);
	ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName4, destToken4, workerDestUid4);
	// Sets the destination channel information.
	mediaRelayConfiguration.setDestChannelInfo(destChannelInfo.channelName,destChannelInfo);
	int result = worker().getRtcEngine().updateChannelMediaRelay(mediaRelayConfiguration);
	```

<div class="alert note">After calling the <code>startChannelMediaRelay</code> method, you can call the <code>updateChannelMediaRelay</code> method to add or delete the relay channels.</div>

We provide an open-source [Cross-Channel-OpenLive-Android](https://github.com/AgoraIO/Advanced-Video/tree/dev/backup/Cross-Channel/Cross-Channel-OpenLive-Android) demo project that implements channel media relay on GitHub. You can try the demo and view the source code.


### API Reference

- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6f09ba685f8ab01d7dc06173286950f6)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#abd40d706379d27cf617376a504f394bd)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0f9f19e48c21190dd4e697dec632c328)
- [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89fd95b3536e8e6afd5f001926162f66)
- [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a6fe2367e9ea61e48a4cc3b373d198b54)

## Considerations

- The Agora RTC SDK supports relaying media streams to a maximum of four destination channels. To add or delete a destination channel, call `updateChannelMediaRelay`.
- This feature supports integer user IDs only.




- When setting the souce channel information (`setSrcChannelInfo`), ensure that you set `uid` as 0, and the `uid` that you use to generate the token should also be set as 0.

- To call `startChannelMediaRelay` again after it succeeds, you must call `stopChannelMediaRelay` to quit the current relay.
