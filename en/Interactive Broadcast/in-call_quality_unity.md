---
title: Report In-call Statistics
description: In-call quality for Android
platform: Unity
updatedAt: Wed Mar 11 2020 05:48:53 GMT-0500 (EST)
---
# Report In-call Statistics
## Introduction

**After joining the channel**, the SDK triggers the following callbacks related to the call quality **once every two seconds**. You can see the last mile network quality, local statistics, audio quality, and video quality of the current call.

<div class="alert note">Ensure that you implement a video call or an interactive broadcast in your project. For details, see <a href="../../en/Interactive%20Broadcast/start_call_android.md">Start a Call</a> or <a href="../../en/Interactive%20Broadcast/start_live_android.md">Start an Interactive Broadcast</a>.</div>

## Network quality report

The `OnNetworkQualityHandler` callback reports the uplink and downlink last mile network quality of each user/host in the current call, see [Quality Rating](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/namespaceagora_1_1rtc.html#aeaf419fcafaa4d58da8b6d8718e31891) for details. Last mile refers to the network from your device to Agora’s edge server. The uplink last mile quality rating is based on the actual transmission bitrate, the uplink network packet loss rate, the average round-trip delay, and the uplink network jitter; while the downlink last mile quality rating is based on the downlink network packet loss rate, the average round-trip delay, and the downlink network jitter.

> - In the communication profile, you receive network quality reports of all the users (including yours) in the channel once every two seconds.
> - In the live broadcast profile, if you are the host, you receive network quality reports of all hosts (including yours) in the channel once every two seconds; if you are the audience, you receive the report of all hosts and yourself once every two seconds.
> <a name="RTT"></a>
> - The higher the ratio of the actual transmission bitrate to the target transmission bitrate, the better the call quality and the higher the network quality.
> - **The average round-trip delay** refers to the average value of multiple round-trip delays in the reported interval.

## Statistics report

The `OnRtcStatsHandler` callback reports call statistics. You can see the duration, the number of users in the channel, the system CPU usage, the application CPU usage, and the following parameters of the current call.

| Parameter                           | Description                                                  | Comment                                                      |
| :---------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `txBytes`/`rxBytes`                 | The total number of bytes sent/received.                     | The number of bytes accumulated since joining the channel.   |
| `txAudioBytes`/`rxAudioByte`        | The total number of audio bytes sent/received.               | The number of bytes accumulated since joining the channel.   |
| `txVideoBytes`/`rxVideoBytes`       | The total number of video bytes sent/received.               | The number of bytes accumulated since joining the channel.   |
| `txKBitRate`/`rxKBitRate`           | The bitrate sent/received.                                   | The actual bitrate sent/received in the reported interval.   |
| `txAudioKBitRate`/`rxAudioKBitRate` | The bitrate sent/received of the audio packet.               | The actual bitrate sent/received in the reported interval.   |
| `txVideoKBitRate`/`rxVideoKBitRate` | The bitrate sent/received of the video packet.               | The actual bitrate sent/received in the reported interval.   |
| `lastmileDelay`                     | The network delay from the local client to Agora’s edge server. | <li>This refers to half of [the average round-trip delay](#RTT), and not the **one-way** network delay from the client to Agora’s edge server or vice versa. <li>The network delay here does not distinguish between the audio and video delay, and is the data obtained by the UDP packet. |
| `txPacketLossRate`                  | The packet loss rate from the local client to Agora’s edge server. | <li>The larger value between audio’s and video’s uplink packet loss rate. <li>The packet loss rate before using the **anti-packet-loss** method. |
| `rxPacketLossRate`                  | The packet loss rate from Agora’s edge server to the local client. | <li>The larger value between audio’s and video’s downlink packet loss rate.  <li>The packet loss rate before using the **anti-packet-loss** method. |

## Audio quality report

### Statistics of local audio streams

The `OnLocalAudioStatsHandler` callback reports the statistics of the audio streams sent. You can see the number of channels (mono or dual), the sample rate, and the **average** sending bitrate in the reported interval.

> The SDK triggers this callback once every two seconds. The sample rate refers to the actual sample rate of the audio streams sent in the reported interval. 

<a name="32"></a>
### State changes of local audio streams

When the state of the local audio stream changes (including the state of the audio recording and encoding), the SDK triggers the `OnLocalAudioStateChangedHandler` callback to report the current state. You can troubleshoot with the error code when exceptions occur.

### Statistics of remote audio streams

![](https://web-cdn.agora.io/docs-files/1565945275984)

The `OnRemoteAudioStatsHandler` callback reports the audio statistics of each remote user/host in the current call. You can see the quality of the audio stream sent by each remote user/host (see [Quality Rating](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/namespaceagora_1_1rtc.html#aeaf419fcafaa4d58da8b6d8718e31891)), the number of channels (mono or dual), and the following parameters.

| Parameter               | Description                                                  | Comment                                                      |
| :---------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `networkTransportDelay` | The network delay from the sender to the receiver.           | Stages 2 + 3 + 4 in the figure above                         |
| `jitterBufferDelay`     | The network delay from the receiver to the network jitter buffer. | Stage 5 in the figure above                                  |
| `audioLossRate`         | The frame loss rate of the received remote audio streams in the reported interval. | <li>Stages 2 + 3 + 4 + 5 in the figure above<li>In a reported interval, audio **freeze** occurs when the audio frame loss rate reaches 4%. |
| `receivedSampleRate`    | The sample rate of the received remote audio streams in the reported interval. |                                                              |
| `receivedBitrate`       | The **average** bitrate of the received remote audio streams in the reported interval. |                                                              |
| `totalFrozenTime`       | The total **freeze** time (ms) of the remote audio streams after the remote user joins the channel. | <li>Agora defines `totalFrozenTime` = The number of times the audio freezes × 2 × 1000 (ms).<li>The total time is the cumulative duration after the remote user joins the channel. |
| `frozenRate`            | The total audio freeze time as a percentage of the total time when the audio is available. | When the remote user/host neither stops sending the audio stream nordisables the audio module after joining the channel, the audio is **available**. |

The `OnRemoteAudioStatsHandler` callback reports statistics more closely linked to the real-user experience of the audio transmission quality. Even if network packet loss occurs, users may find the overall audio quality acceptable because the audio frame loss rate of the received audio streams may not be high due to the **anti-packet-loss** and congestion control methods, such as forward error correction (FEC), retransmissions and bandwidth estimation.

> - In the communication profile, you receive the audio stream statistics of all the remote users (excluding yours) in the channel once every two seconds.
> - In the live broadcast profile, if you are the host, you receive the audio stream statistics of all remote hosts (excluding yours) in the channel once every two seconds; if you are the audience, you receive the statistics of all hosts in the channel once every two seconds.
> - Agora's **audio module** refers to the audio processing process, and not the actual module in the SDK. When sending audio streams, the audio module refers to the processes of audio sampling, pre-processing, and encoding; when receiving audio streams, the audio module refers to the processes of audio decoding, post-processing, and playback.
> - Users can only turn on/off their own audio modules.
> - By default, the audio freezes at most once in each reported interval.
### <a name="34"></a>State changes of remote audio streams 

When the state of **remote** audio stream changes, the SDK triggers the `OnRemoteAudioStateChangedHandler` callback to report the current state and the reason for the state change.

> - In the communication profile, this callback reports to you the audio stream state information of all the remote users (excluding yours) in the channel once every two seconds.
> - In the live broadcast profile, if you are the host, this callback reports to you the audio stream state information of all the remote hosts(excluding yours) in the channel once every two seconds; if you are the audience, this callback reports to you the audio stream state information of all the remote hosts in the channel once every two seconds.

## Video quality report

### Statistics of local video streams

The `OnLocalVideoStatsHandler` callback reports the statistics of the video streams sent. You can see the dimensions of the encoding frame and the following parameters.

> If you have called the `EnableDualStreamMode` method to enable [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode), this callback reports the statistics of the high-video streams sent.

| Parameter                 | Description                                                  | Comment                                                      |
| :------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `rendererOutputFrameRate` | The output frame rate of the local video renderer.           |                                                              |
| `encoderOutputFrameRate`  | The output frame rate of the local video encoder.            |                                                              |
| `targetBitrate`           | The target bitrate of the current encoder.                   | This value is estimated by the SDK based on current network conditions. |
| targetFrameRate           | The target frame rate of the current encoder.                |                                                              |
| `encodedBitrate`          | The bitrate of the encoding video.                           | Does not include the bitrate of the retransmission videos.   |
| `sentBitrate`             | The bitrate of the video sent in the reported interval.      | Does not include the bitrate of the retransmission videos.   |
| `sentFrameRate`           | The frame rate of the video sent in the reported interval.   | Does not include the frame rate of the retransmission videos. |
| `encodedFrameCount`       | The total frames of the video sent.                          | The number of frames accumulated since joining the channel.  |
| `codecType`               | The codec type of the local video.                           | <li>`VIDEO_CODEC_VP8 = 1`: VP8<li>`VIDEO_CODEC_H264 = 2`: (Default) H.264 |
| `qualityAdaptIndication`  | The local video quality change in terms of `targetBitrate` and `targetFrameRate` in thisreported interval. | Compared to the video quality in the last statistics (two seconds ago), the video quality change in this reported interval:<li>The quality stays the same.<li>The quality improves.<li>The quality deteriorates. |

### <a name="42"></a>State changes of local video streams

When the state of the local video changes, the SDK triggers the `OnLocalVideoStateChangedHandler` callback to report the current state. You can troubleshoot with the error code when exceptions occur.

### Statistics of remote video streams

![](https://web-cdn.agora.io/docs-files/1565945292345)

The `OnRemoteVideoStatsHandler` callback reports the video statistics of each remote user/host in the current call. You can see their video dimensions and the following parameters.

| Parameter                 | Description                                                  | Comment                                                      |
| :------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `rxStreamType`            | The type of video streams.                                   | High-video streams or low-video streams, see [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode). |
| `receivedBitrate`         | The bitrate of the video received in the reported interval.  |                                                              |
| `packetLossRate`          | The packet loss rate of the video received in the reported interval. | <li>Stages 2 + 3 + 4 in the figure above<li>The packet loss rate after using the **anti-packet-loss** method, which is lower than before. |
| `decoderOutputFrameRate`  | The output frame rate of the remote video decoder.           |                                                              |
| `rendererOutputFrameRate` | The output frame rate of the remote video renderer.          |                                                              |
| `totalFrozenTime`         | The total **freeze** time (ms) of the remote video stream after the remote user joins the channel. | In a video call or video broadcasting session where the frame rate is set to no less than 5 fps, video **freeze** occurs when the time interval between two adjacent renderable video frames is more than 500 ms. |
| `frozenRate`              | The total video freeze time as a percentage of the total time when the video is available. | When the remote user/host neither stops sending the video stream nor disables the video module after joining the channel, the video is **available**. |

> - In the communication profile, you receive video stream statistics of all the remote users (excluding yours) in the channel once every two seconds.
> - In the live broadcast profile, if you are the host, you receive video stream statistics of all the remote hosts (excluding yours) in the channel once every two seconds; if you are the audience, you receive the statistics for all the hosts in the channel once every two seconds.
> - Agora’s **video module** refers to the video processing process, and not the actual module in the SDK. When sending video streams, the video module refers to the processes of video capturing, pre-processing, and encoding; when receiving video streams, the video module refers to the processes of video decoding, post-processing, and rendering/playing.
> - Users can only turn on/off their own video modules.

### <a name="44"></a>State changes of remote video streams

When the state of remote video streams changes, the SDK triggers the `OnRemoteVideoStateChangedHandler` callback to report the current state and the reason for the state change.

> - In the communication profile, this callback reports to you the video stream state information of all the remote users (excluding yours) in the channel once every two seconds.
> - In the live broadcast profile, if you are the host, this callback reports to you the video stream state information of all the remote hosts (excluding yours) in the channel once every two seconds; if you are the audience, this callback reports to you the video stream state information of all the remote hosts in the channel once every two seconds.

## API reference

- [`OnNetworkQualityHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a1c331efe111ae4b958dc64a6f12ecf8a)
- [`OnRtcStatsHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a931fc3d5383ece93efe5bb97ab7271f4)
- [`OnLocalAudioStatsHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a5f1f0ac357fa5985af4f2cf7d76e26bf)
- [`OnLocalAudioStateChangedHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a6d3dde84ac7c4c9bd5cb290c2ab717b2)
- [`OnRemoteAudioStatsHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a2c15901cdd77defbeb01709e5c954d9a)
- [`OnRemoteAudioStateChangedHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a92f016ea980eba06cf38192191d17e01)
- [`OnLocalVideoStatsHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a40eab1be7af0f19840212bf47aeae841)
- [`OnLocalVideoStateChangedHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#ae677bebf52f50d861389acec8197b771)
- [`OnRemoteVideoStatsHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a4a0e9e6402f4e279446a0e048423e30d)
- [`OnRemoteVideoStateChangedHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/namespaceagora__gaming__rtc.html#a53d577f5c1c2d863f7946d86d76adb13)

## Considerations

The SDK does not trigger the [`OnLocalAudioStateChangedHandler`](#32), [`OnRemoteAudioStateChangedHandler`](#34), [`OnLocalVideoStateChangedHandler`](#42), and [`OnRemoteVideoStateChangedHandler`](#44) callbacks once every two seconds. See their respective trigger conditions on this page.
