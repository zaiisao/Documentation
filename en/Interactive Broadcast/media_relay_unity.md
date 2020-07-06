
---
title: Co-host across Channels
description: 
platform: Unity
updatedAt: Mon Jul 06 2020 10:35:18 GMT+0800 (CST)
---
# Co-host across Channels
## Introduction
Co-hosting across channels refers to the process where the Agora SDK relays the media stream of a host from a live interactive streaming channel (source channel) to a maximum of four live interactive streaming channels (destination channels). It has the following benefits:

- All hosts in the relay channels can see and hear each other.
- The audiences in the relay channels can see and hear all hosts.

Co-hosting across channels applies to scenarios such as an online singing contest, where hosts of different channels interact with each other.

## Implementation

<div class="alert note">To enable channel media relay, contact <a href="mailto:support@agora.io">support@agora.io</a>.</div>

Before relaying media streams across channels, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_unity.md).

The Agora Unity SDK support co-hosting across channels with the following methods:

- `StartChannelMediaRelay`
- `UpdateChannelMediaRelay`
- `StopChannelMediaRelay`

During the relay, the SDK reports the states and events of the channel media relay with the `OnChannelMediaRelayStateChangedHandler` and `OnChannelMediaRelayEventHandler` callbacks. Refer to the following codes and their corresponding states to implement your code logic:

| State codes | Event codes | The media stream relay state |
| ---------------- | ---------------- | ---------------- |
| RELAY_STATE_RUNNING(2) and RELAY_OK(0)      | RELAY_EVENT_PACKET_SENT_TO_DEST_CHANNEL(4)     | The channel media relay starts.      |
| RELAY_STATE_FAILURE(3)      | /     | Exceptions occur for the media stream relay. Refer to the error parameter for troubleshooting.      |
| RELAY_STATE_IDLE(0) and RELAY_OK(0)      | /     | The channel media relay stops.      |

**Note**:
- Any host in a Live Broadcast channel can call the `StartChannelMediaRelay` method to enable channel media stream relay. The SDK relays the media streams of the host who calls the method.
- During the media stream relay, if the host of the destination channel drops offline or leaves the channel, the host of the source channel receives the `OnUserOfflineHandler` callback.

### API call sequence

Follow the API call sequence to implement your code logic:

![](https://web-cdn.agora.io/docs-files/1588233232545)

### Sample code

Starts the channel media relay:

```c#
ChannelMediaInfo srcChannelInfo = new ChannelMediaInfo(srcChannelName, srcToken, workerSrcUid);   
ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName, destToken, destUid);
ChannelMediaRelayConfiguration mediaRelayConfiguration = new ChannelMediaRelayConfiguration();
// Sets the source channel information.
mediaRelayConfiguration.srcInfo = srcChannelInfo;
// Sets the destination channel information.
mediaRelayConfiguration.destInfos = destChannelInfo;
mRtcEngine.StartChannelMediaRelay(mediaRelayConfiguration);
```

Adds or deletes the media relay channels:

```c#
ChannelMediaInfo srcChannelInfo = new ChannelMediaInfo(srcChannelName, srcToken, workerSrcUid);   
ChannelMediaInfo destChannelInfo = new ChannelMediaInfo(destChannelName, destToken, destUid);
ChannelMediaRelayConfiguration mediaRelayConfiguration = new ChannelMediaRelayConfiguration();
// Sets the source channel information.
mediaRelayConfiguration.srcInfo = srcChannelInfo;
// Sets the destination channel information.
mediaRelayConfiguration.destInfos = destChannelInfo;
mRtcEngine.UpdateChannelMediaRelay(mediaRelayConfiguration);
```

<div class="alert note">After calling the <code>StartChannelMediaRelay</code> method, you can call the <code>UpdateChannelMediaRelay</code> method to add or delete the relay channels.</div>

### API reference

- [`StartChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a16d12d6d67882c9689220d48116c6327)
- [`UpdateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a8dd41b43195f309d9d1d9f20e70f3482)
- [`StopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ae6cdbbb3bfc698f9b85147904209255c)
- [`OnChannelMediaRelayStateChangedHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a3ea70770219197c5ba562d5c3333cbbc)
- [`OnChannelMediaRelayEventHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#aff4b411469723353639319e9811edcff)

## Considerations

- The Agora RTC SDK supports relaying media streams to a maximum of four destination channels. To add or delete a destination channel, call `UpdateChannelMediaRelay`.
- This feature supports integer user IDs only.
- When setting the souce channel information (`srcInfo`), ensure that you set `uid` as `0`, and the `uid` that you use to generate the token should also be set as `0`.
- To call `StartChannelMediaRelay` again after it succeeds, you must call `StopChannelMediaRelay` to quit the current relay.
