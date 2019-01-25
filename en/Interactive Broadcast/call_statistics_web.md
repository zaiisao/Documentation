
---
title: Report Call Statistics
description: 
platform: Web
updatedAt: Fri Jan 25 2019 01:24:47 GMT+0000 (UTC)
---
# Report Call Statistics
# Report In-call Statistics

## Introduction

The Agora Web SDK provides API methods for you to get the audio-and-video statistics reflecting the overall quality of a call. 

The statistics include:

- [The statistics of the system](#system_statistics) (battery level).
- [The statistics of the network](#network_statistics) (network type and network connection).
- [The statistics of the remote stream](#local_stream_statistics).
- [The statistics of the local stream](#remote_stream_statistics).

## Implementation

Before proceeding, ensure that you prepare the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md).

<a name ="system_statistics"></a>
### Get the statistics of the system

You can use the [`Client.getSystemStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getsystemstats) method to get the statistics of the system for optimizing your application. Currently, only the battery level information is provided.

```javascript
client.getSystemStats((stats) => {
	console.log(`Current battery level: ${stats.BatteryLevel}`);
});
```

> This method is under testing. For web browser compatibility, see [Battery Status API](https://developer.mozilla.org/en-US/docs/Web/API/Battery_Status_API).

<a name ="network_statistics"></a>
### Get the statistics of the network

The statistics of the network include the network type and the network connection. You can use these statistics to optimize your application to provide customized content.

You can:  

- Use the [`Client.getNetworkStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getnetworkstats) method to get the network type, including:
  - `bluetooth`: Bluetooth network.
  - `cellular`: Cellular network.
  - `ethernet`: Ethernet.
  - `none`: No network.
  - `wifi`: Wi-Fi.
  - `wimax`: WiMax.
  - `other`: Other network types.
  - `unknown`: Unknown network types.
  - `UNSUPPORTED`: The web browser does not support getting the network type.

```javascript 
client.getNetworkStats((stats) => {                        
	console.log(`Current network type: ${stats.NetworkType}`); 
});                                                        
```

- Use the [`Client.getTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#gettransportstats) method to get the statistics of the network connection, including:
  - `RTT`: RTT (Round-trip time) (ms) between the Agora Web SDK and the access node of the Agora SD-RTN.

```javascript
client.getTransportStats((stats) => {
	console.log(`Current Transport RTT: ${stats.RTT}`);
});                           
```

> - The `Client.getNetworkStats` method requires Google Chrome 61 or later and compatibility is not guaranteed. See [Network Information API](https://developer.mozilla.org/en-US/docs/Web/API/Network_Information_API).
> - It takes less than 3 seconds to get the statistics of the `Client.getTransportStats` method.

<a name ="local_stream_statistics"></a>
### Get the statistics of the local stream

You can: 

- Use the [`Client.getLocalAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getlocalaudiostats) method to get the audio statistics of the published stream, including the:
  - `CodecType`: Encoding type of the sent audio.
  - `MuteState`: Whether or not the audio is muted.
  - `RecordingLevel`: Frequency of the captured audio.
  - `SamplingRate`: Sample rate (kHz) of the sent audio.
  - `SendBitrate`: Bitrate (Kbps) of the sent audio.
  - `SendLevel`: Frequency of the sent audio.

```javascript
client.getLocalAudioStats((localAudioStats) => {
	for(var uid in localAudioStats){
		console.log(`Audio CodecType from ${uid}: ${localAudioStats[uid].CodecType}`);
		console.log(`Audio MuteState from ${uid}: ${localAudioStats[uid].MuteState}`);
		console.log(`Audio RecordingLevel from ${uid}: ${localAudioStats[uid].RecordingLevel}`);
		console.log(`Audio SamplingRate from ${uid}: ${localAudioStats[uid].SamplingRate}`);
		console.log(`Audio SendBitrate from ${uid}: ${localAudioStats[uid].SendBitrate}`);
		console.log(`Audio SendLevel from ${uid}: ${localAudioStats[uid].SendLevel}`);
	}
		});
```

- Use the [`Client.getLocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getlocalvideostats) method to get the video statistics of the published stream, including the:
  - `EncodeDelay`: Delay (ms) from capturing to encoding the local video.
  - `MuteState`: Whether or not the video is muted.
  - `SendBitrate`: Bitrate (Kbps) of the sent video.
  - `SendFrameRate`: Frame rate (fps) of the sent video.
  - `SendResolutionHeight`: Height (pixel) of the sent video.
  - `SendResolutionWidth`: Width (pixel) of the sent video.
  - `TargetSendBitrate`: Bitrate (Kbps) of the local video set in the `setVideoProfile` method.

```javascript
client.getLocalVideoStats((localVideoStats) => {
	for(var uid in localVideoStats){
		console.log(`Video EncodeDelay from ${uid}: ${localVideoStats[uid].EncodeDelay}`);
		console.log(`Video MuteState from ${uid}: ${localVideoStats[uid].MuteState}`);
		console.log(`Video SendBitrate from ${uid}: ${localVideoStats[uid].SendBitrate}`);
		console.log(`Video SendFrameRate from ${uid}: ${localVideoStats[uid].SendFrameRate}`);
		console.log(`Video SendResolutionHeight from ${uid}: ${localVideoStats[uid].SendResolutionHeight}`);
		console.log(`Video SendResolutionWidth from ${uid}: ${localVideoStats[uid].SendResolutionWidth}`);
		console.log(`Video TargetSendBitrate from ${uid}: ${localVideoStats[uid].TargetSendBitrate}`);
	}
});
```

> Call the `getLocalAudioStats` and `getLocalVideoStats` methods after receiving the `stream-published` callback. It takes less than 3 seconds to get the statistics.

<a name ="remote_stream_statistics"></a>
### Get the statistics of the remote stream

You can: 

- Use the [`Client.getRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremoteaudiostats) method to get the **audio** statistics of the remote stream, including the：
  - `CodecType`: Decoding type of the received audio.
  - `End2EndDelay`: End-to-end delay (ms). The delay from capturing to playing the audio.
  - `MuteState`: Whether or not the audio is muted.
  - `PacketLossRate`: Packet loss rate (%) of the remote audio.
  - `RecvBitrate`: Bitrate (Kbps) of the received audio.
  - `RecvLevel`: Volume of the received audio.
  - `TransportDelay`: Transport delay (ms). The delay from sending to receiving the audio.

```javascript
client.getRemoteAudioStats((remoteAudioStatsMap) => {
	for(var uid in remoteAudioStatsMap){
		console.log(`Audio CodecType from ${uid}: ${remoteAudioStatsMap[uid].CodecType}`);
		console.log(`Audio End2EndDelay from ${uid}: ${remoteAudioStatsMap[uid].End2EndDelay}`);
		console.log(`Audio MuteState from ${uid}: ${remoteAudioStatsMap[uid].MuteState}`);
		console.log(`Audio PacketLossRate from ${uid}: ${remoteAudioStatsMap[uid].PacketLossRate}`);
		console.log(`Audio RecvBitrate from ${uid}: ${remoteAudioStatsMap[uid].RecvBitrate}`);
		console.log(`Audio RecvLevel from ${uid}: ${remoteAudioStatsMap[uid].RecvLevel}`);
		console.log(`Audio TransportDelay from ${uid}: ${remoteAudioStatsMap[uid].TransportDelay}`);
	}
});
```

- Use the [`Client.getRemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremotevideostats) method to get the video statistics of the remote stream, including the: 
  - `End2EndDelay`: End-to-end delay (ms). The delay from capturing to playing the video.
  - `MuteState`: Whether or not the video is muted.
  - `PacketLossRate`: Packet loss rate (%) of the remote video.
  - `RecvBitrate`: Bitrate (Kbps) of the received video.
  - `RecvResolutionHeight`: Height (pixel) of the received video.
  - `RecvResolutionWidth`: Width (pixel) of the received video.
  - `RenderFrameRate`: Rendering frame rate (fps) of the decoded video.
  - `TransportDelay`: Transport delay (ms). The delay from sending to receiving the video.

```javascript
client.getRemoteVideoStats((remoteVideoStatsMap) => {
	for(var uid in remoteVideoStatsMap){
		console.log(`Vodeo End2EndDelay from ${uid}: ${remoteVideoStatsMap[uid].End2EndDelay}`);
		console.log(`Vodeo MuteState from ${uid}: ${remoteVideoStatsMap[uid].MuteState}`);
		console.log(`Vodeo PacketLossRate from ${uid}: ${remoteVideoStatsMap[uid].PacketLossRate}`);
		console.log(`Vodeo RecvBitrate from ${uid}: ${remoteVideoStatsMap[uid].RecvBitrate}`);
		console.log(`Vodeo RecvResolutionHeight from ${uid}: ${remoteVideoStatsMap[uid].RecvResolutionHeight}`);
		console.log(`Vodeo RecvResolutionWidth from ${uid}: ${remoteVideoStatsMap[uid].RecvResolutionWidth}`);
		console.log(`Vodeo RenderFrameRate from ${uid}: ${remoteVideoStatsMap[uid].RenderFrameRate}`);
		console.log(`Vodeo TransportDelay from ${uid}: ${remoteVideoStatsMap[uid].TransportDelay}`);
	}
});
```

> Call the `Client.getRemoteAudioStats` and `Client.getRemoteVideoStats` methods after receiving the `stream-subscribed` callback. It takes less than 3 seconds to get the statistics.

