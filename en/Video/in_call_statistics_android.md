
---
title: Report In-call Statistics
description: 
platform: Android
updatedAt: Fri Aug 16 2019 09:23:26 GMT+0800 (CST)
---
# Report In-call Statistics
The in-call audio-and-video statistics reflect the overall quality of a call after the SDK joins a channel and are reported once every two seconds.

The statistics include the:
- Uplink and downlink ratings of each user in the channel.
- Video statistics of the **local** user.
  - Sent frame rate and bitrate.
- Audio statistics of the **remote** users:
  - End-to-end audio statistics.
  - Transport-layer audio statistics.
- Video statistics of the **remote** users:
  - End-to-end video statistics.
  - Transport-layer video statistics.

## Uplink and Downlink Ratings of Each User in the Channel

![](https://web-cdn.agora.io/docs-files/1546918207650)

### Feature Description

After joining a channel, you can get the ratings of the uplink or downlink last-mile network quality of each user/host in a channel through the `onNetworkQuality` callback.  

The `onNetworkQuality` callback uses the `uid` parameter. If a channel has multiple users/hosts, the SDK triggers this callback as many times. The ratings include: 

- `txQuality`: The uplink last-mile (from the device to the Agora edge server) network quality rating (EXCELLENT~VBAD) <sup>[1]</sup> in terms of:
  - The ratio of the sent uplink video bitrate to the target uplink bitrate. The higher the ratio, the higher the quality. 
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

[`onNetworkQuality`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a76be982389183c5fe3f6e4b03eaa3bd4)

```java
void onNetworkQuality(int uid, int txQuality, int rxQuality) {
    }
```

### Considerations

Here are the differences between the `onNetworkQuality` and `onLastmileQuality` callbacks:

`onNetworkQuality`:
- The SDK triggers the `onNetworkQuality` callback after a user joins a channel.
- The `onNetworkQuality` reports the uplink and downlink last-mile quality between the device of each user/host in a channel and Agora's edge server.
- If a channel has multiple channels, the SDK triggers the `onNetworkQuality` callback as many times. 

`onLastmileQuality`:
- The SDK triggers the `onLastmileQuality` callback when a user calls the `enableLastmileTest` method before joining a channel.
- The `onLastmileQuality` callback reports the uplink and downlink last-mile quality between the local device and Agora's edge server.

## Video Statistics of the Local User

### Feature Description

This feature reports the video quality of the local video stream.

- `onLocalVideoStats`: The SDK triggers this callback once every two seconds to report the video quality of the local video stream. This callback returns the following information: 
  - `sentBitrate`: The average sending bitrate (Kbps) since the last count.
  - `sentFrameRate`: The average sending frame rate (fps).
  - `targetBitrate`: The target bitrate (Kbps) of the current encoder. This value is estimated by the SDK based on the current network conditions.
  - `targetFrameRate`: The target frame rate (fps) of the current encoder.
  - `qualityAdaptIndication`: The adaptation of the local video (mainly the bitrate or the frame rate) compared with the statistics of the last count:
	- `ADAPT_NONE` = 0: No adaptation for the local video bitrate and frame rate.
	- `ADAPT_UP_BANDWIDTH` = 1: The local video bitrate or frame rate increases due to a bandwidth increase.
	- `ADAPT_DOWN_BANDWIDTH` = 2: The local video bitrate or frame rate decreases due to a bandwidth decrease.

### API Reference

[`onLocalVideoStats`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a79fb5d32bb694d24b54a523d924dc7ef)

```java
void onLocalVideoStats(LocalVideoStats stats) {
    }
```

## Audio Statistics of the Remote Users

![](https://web-cdn.agora.io/docs-files/1546918221116)

### Feature Description

As shown in the figure above, this feature reports the end-to-end audio quality and of the transport layer.

- `onRemoteAudioStats`: The SDK triggers this callback once every two seconds to report the end-to-end quality of a remote audio stream that is closely linked to the real-user experience. This callback returns the following information: 
  - `quality`: Audio quality rating at the receiver's end (EXCELLENT～VBAD) <sup>[2]</sup>.
  - `networkTransportDelay`: Network delay in the transport layer (ms); `networkTransportDelay` = Delay 2 + Delay 3 + Delay 4.
  - `jitterBufferDelay`: Jitter buffer delay at the receiver's end (ms); `jitterBufferDelay` = Delay 5.
  - `audioLossRate`: Audio packet loss rate (%).
- `onRemoteAudioTransportStats`: The SDK triggers this callback once every two seconds to report the transport layer quality of a remote audio stream. This callback returns the following information: 
  - `delay`: Network delay in the transport layer (ms); `delay` = Delay 2 + Delay 3 + Delay 4.
  - `lost`: Audio packet loss rate in the transport layer (%); `lost` = (packetLoss 2 + packetLoss 3 + packetLoss 4) / totalPacketsSent.
  - `rxKBitRate`: Received audio bitrate (Kbps).

> [2] See the [quality rating table](#table). 

### API Reference

- [`onRemoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html?transId=250ddca0-0759-11e9-aad5-0347068be2f5#a9eaf8021d6f0c97d056e400b50e02d54)

```java
void onRemoteAudioStats(RemoteAudioStats stats) {
    }
```

- [`onRemoteAudioTransportStats`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a826009699e73d5225d4ce9e3a29b91f4)

```
void onRemoteAudioTransportStats(int uid, int delay, int lost, int rxKBitRate) {
    }
```


### Considerations

Here is the difference between the `onRemoteAudioStats` and `onRemoteAudioTransportStats` callbacks:

- The `onRemoteAudioTransportStats` callback reports the network quality between the Agora edge servers at the two ends with objective audio statistics, such as the packet loss rate and network delay. 
- The `onRemoteAudioStats` callback reports the overall audio quality from end to end that is closely linked to the real-user experience. Schemes such as FEC (Forward Error Correction) or retransmission counter the frame loss rate. Hence, users may find the overall audio quality acceptable even when the packet loss rate is high. 

## Video Statistics of the Remote Users

![](https://web-cdn.agora.io/docs-files/1546918238110)

### Feature Description

As shown in the figure above, this feature reports the video quality of the end-to-end and transport layer.

- `onRemoteVideoStats`: The SDK triggers this callback once every two seconds to report the end-to-end quality of a remote video stream that is closely linked to the real-user experience. This callback returns the following information:
  - `receivedBitrate`: Received bitrate (Kbps).
  - `receivedFrameRate`: Received frame rate (fps).
  - `rxStreamType`: Received stream type.
- `onRemoteVideoTransportStats`: The SDK triggers this callback once every two seconds to report the transport layer quality of a remote video stream. This callback returns the following information: 
  - `delay`: Network delay in the transport layer (ms); `delay` = Delay 2 + Delay 3 + Delay 4.
  - `lost`: Video packet loss rate in the transport layer (%); `lost` = (packetLoss 2 + packetLoss 3 + packetLoss 4) / totalPacketsSent.
  - `rxKBitRate`: Received video bitrate (Kbps).

### API Reference

- [`onRemoteVideoStats`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#abb7af6e2827bbd03c6ab8338a0f616ca)
```java
void onRemoteVideoStats(RemoteVideoStats stats) {
    }
```
- [`onRemoteVideoTransportStats`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8e8bea20663388c250b299641b25ade9)
```java
void onRemoteVideoTransportStats(int uid, int delay, int lost, int rxKBitRate) {
    }
```

### Considerations

Here is the difference between the `onRemoteVideoTransportStats` and `onRemoteVideoStats` callbacks:

- The `onRemoteVideoTransportStats` callback reports the network quality between the Agora edge servers at the two ends with objective video statistics, such as the packet loss rate and network delay. 
- The `onRemoteVideoStats` callback reports the overall video quality from end to end that is closely linked to the real-user experience. 


