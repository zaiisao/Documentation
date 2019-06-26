
---
title: Report In-call Statistics
description: 
platform: macOS
updatedAt: Wed Jun 26 2019 05:59:57 GMT+0800 (CST)
---
# Report In-call Statistics
The in-call audio statistics reflect the overall quality of a call after the SDK joins a channel and are reported once every two seconds.

The statistics include the:

* Uplink and downlink ratings of each user in the channel.
* Audio statistics of the remote users:
	* End-to-end audio statistics.
	* Transport-layer audio statistics.

## Uplink and Downlink Ratings of Each User in the Channel

![](https://web-cdn.agora.io/docs-files/1546918316091)

### Feature Description

After joining a channel, you can get the ratings of the uplink or downlink last-mile network quality of each user in a channel through the `networkQuality` callback.

The `networkQuality` callback uses the `uid` parameter. If a channel has multiple users, the SDK triggers this callback as many times. The ratings include: 

- `txQuality`: The uplink last-mile (from the device to the Agora edge server) network quality rating (EXCELLENT~VBAD) <sup>[1]</sup> in terms of:
  - The ratio of the sent uplink bitrate to the target uplink bitrate. The higher the ratio, the higher the quality. 
  - The uplink packet loss rate.
  - The average round-trip time (RTT).
  - The uplink jitter.
- `rxQuality`: The downlink last-mile network quality rating (EXCELLENT~VBAD) <sup>[1]</sup> in terms of:
  - The downlink packet loss rate. 
  - The average round-trip time (RTT).
  - The downlink jitter.

<a name ="table"></a>
> [1] Quality Rating Table:
>
> | Rating | Description                                                  |
> | ------ | :----------------------------------------------------------- |
> | 0      | UNKNOWN: The network quality is unknown.                     |
> | 1      | EXCELLENT: The network quality is excellent.                 |
> | 2      | GOOD: The network quality is quite good, but the bitrate may be slightly lower than excellent. |
> | 3      | POOR: Users can feel the communication slightly impaired.    |
> | 4      | BAD: Users cannot communicate smoothly.                      |
> | 5      | VBAD: The network is so bad that users can barely communicate. |
> | 6      | DOWN: The network is disconnected and users cannot communicate at all.|

### API Reference

[`networkQuality`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkQuality:txQuality:rxQuality:)

```objective-c
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine networkQuality:(NSUInteger)uid txQuality:(AgoraNetworkQuality)txQuality rxQuality:(AgoraNetworkQuality)rxQuality
```

### Considerations

Here are the differences between the `networkQuality` and `lastmileQuality` callbacks:

`networkQuality`:
- The SDK triggers the `networkQuality` callback after a user joins a channel. 
- The `networkQuality` callback reports the uplink and downlink last-mile quality between the device of each user in a channel and Agora's edge server.
- If a channel has multiple channels, the SDK triggers the `networkQuality` callback as many times. 

`lastmileQuality`:
- The SDK triggers the `lastmileQuality` callback when a user calls the `enableLastmileTest` method before joining a channel.
- The `lastmileQuality` callback reports the uplink and downlink last-mile quality between the local device and Agora's edge server.

## Audio Statistics of the Remote Users

![](https://web-cdn.agora.io/docs-files/1546918332341)

### Feature Description

As shown in the figure above, this feature reports the end-to-end audio quality and of the transport layer.

- `remoteAudioStats`: The SDK triggers this callback once every two seconds to report the end-to-end quality of a remote audio stream that is closely linked to the real-user experience. This callback returns the following information: 
  - `quality`: Audio quality rating at the receiver's end（EXCELLENT～VBAD）<sup>[2]</sup>.
  - `networkTransportDelay`: Network delay in the transport layer (ms); `networkTransportDelay` = Delay 2 + Delay 3 + Delay 4.
  - `jitterBufferDelay`: Jitter buffer delay at the receiver's end (ms), `jitterBufferDelay` = Delay 5.
  - `audioLossRate`: Audio packet loss rate (%).
- `audioTransportStatsOfUid`: The SDK triggers this callback once every two seconds to report the transport layer quality of a remote audio stream. This callback returns the following information: 
  - `delay`: Network delay in the transport layer (ms); `delay` = Delay 2 + Delay 3 + Delay 4.
  - `lost`: Audio packet loss rate in the transport layer (%); `lost` = (packetLoss 2 + packetLoss 3 + packetLoss 4) / totalPacketsSent.
  - `rxKBitRate`: Received audio bitrate (Kbps).

> [2] See the [quality rating table](#table). 

### API Reference

- [`remoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)

```objective-c
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine remoteAudioStats:(AgoraRtcRemoteAudioStats *_Nonnull)stats
```

- [`audioTransportStatsOfUid`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:)

```objective-c
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine audioTransportStatsOfUid:(NSUInteger)uid delay:(NSUInteger)delay lost:(NSUInteger)lost rxKBitRate:(NSUInteger)rxKBitRate
```

### Considerations

Here is the difference between the `remoteAudioStats` and `audioTransportStatsOfUid` callbacks:

- The `audioTransportStatsOfUid` callback reports the network quality between the Agora edge servers at the two ends with objective audio statistics, such as the packet loss rate and network delay. 
- The `remoteAudioStats` callback reports the overall audio quality from end to end that is closely linked to the real-user experience. Schemes such as FEC (Forward Error Correction) or retransmission counter the frame loss rate. Hence, users may find the overall audio quality acceptable even when the packet loss rate is high. 

