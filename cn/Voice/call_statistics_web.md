
---
title: 检测通话质量
description: 
platform: Web
updatedAt: Thu Feb 14 2019 04:03:25 GMT+0000 (UTC)
---
# 检测通话质量
## 功能描述

Agora Web SDK 支持获取通话质量相关的统计数据，包括：

- [系统信息](#system_statistics)（系统电量）；
- [网络相关数据](#network_statistics)（网络类型和网络连接质量）；
- [本地发布流质量统计](#local_stream_statistics)；
- [远端订阅流质量统计](#remote_stream_statistics)。

## 实现方法

开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Voice/web_prepare.md)。

<a name ="system_statistics"></a>
### 获取系统信息

Agora Web SDK 通过 [`Client.getSystemStats`](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#getsystemstats) 方法提供系统信息供开发者使用。目前暂只提供系统电量信息。

``` javascript
client.getSystemStats((stats) => {
	console.log(`Current battery level: ${stats.BatteryLevel}`);
});
```

> 该功能处于试验阶段，浏览器兼容信息请参考 [Battery Status API](https://developer.mozilla.org/zh-CN/docs/Web/API/Battery_Status_API)。

<a name ="network_statistics"></a>
### 获取网络相关数据

当前可获取的网络相关数据包括网络类型和网络连接质量。开发可根据这些信息优化应用程序，为用户展现不同清晰度的内容。

Agora Web SDK 提供： 

- [`Client.getNetworkStats`](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#getnetworkstats) 方法获取网络类型，包括：
  - `bluetooth`：蓝牙网络。
  - `cellular`：蜂窝移动数据网络。
  - `ethernet`：以太网。
  - `none`：没有网络。
  - `wifi`：Wi-Fi。
  - `wimax`：WiMax。
  - `other`：其他网络类型。
  - `unknown`：未知网络类型。
  - `UNSUPPORTED`：浏览器不支持获取网络类型。

``` javascript 
client.getNetworkStats((stats) => {                        
	console.log(`Current network type: ${stats.NetworkType}`); 
});                                                        
```

- [`Client.getTransportStats`](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#gettransportstats) 方法获取网络连接状况统计数据，包括：
  - `RTT`：Agora Web SDK 到 Agora SD-RTN 接入节点的平均往返延时（ RTT，Round-Trip Time），单位 ms。

``` javascript
client.getTransportStats((stats) => {
	console.log(`Current Transport RTT: ${stats.RTT}`);
});                           
```

> - `Client.getNetworkStats` 方法仅支持 Chrome 61 及之后版本，且无法保证兼容性，详见[网络状况 API](https://developer.mozilla.org/zh-CN/docs/Web/API/Network_Information_API)。
>
> - `Client.getTransportStats` 方法的部分统计数据可能耗费 0-3 秒时间返回。

<a name ="local_stream_statistics"></a>
### 获取本地发布流质量统计

Agora Web SDK 提供：

- [`Client.getLocalAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#getlocalaudiostats) 方法获取本地发布流的**音频**统计数据，一个 uid 对应一组数据，包括：
  - `CodecType`：音频编码类型。
  - `MuteState`：音频是否静音。
  - `RecordingLevel`：音频采集能量。
  - `SamplingRate`：音频采样率。
  - `SendBitrate`：音频发送码率（Kbps）。
  - `SendLevel`：音频发送能量。

``` javascript
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

- [`Client.getLocalVideoStats`](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#getlocalvideostats) 方法获取本地发布流的**视频**统计数据，一个 uid 对应一组数据，包括：
  - `EncodeDelay`：本地视频从采集到编码的延时（ms）。
  - `MuteState`：视频画面是否开启。
  - `SendBitrate`：视频发送码率（Kbps）。
  - `SendFrameRate`：视频发送帧率（fps）。
  - `SendResolutionHeight`：视频发送分辨率高度，单位为像素。
  - `SendResolutionWidth`：视频发送分辨率宽度，单位为像素。
  - `TargetSendBitrate`：`setVideoProfile` 中设置的目标发送码率（Kbps）。 

``` javascript
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

> `getLocalAudioStats` 和 `getLocalVideoStats` 这两个方法的部分数据需要在 `stream-published` 事件后进行统计，可能耗费 0-3 秒时间返回。

<a name ="remote_stream_statistics"></a>
### 获取远端订阅流质量统计

Agora Web SDK 提供：

- [`Client.getRemoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#getremoteaudiostats) 方法获取远端订阅流的**音频**统计数据，一个 uid 对应一组数据，包括：
  - `CodecType`：音频解码类型。
  - `End2EndDelay`：端到端延时（ms），从远端采集音频到本地播放音频的延时。
  - `MuteState`：音频是否静音。
  - `PacketLossRate`：远端音频的丢包率（%）。
  - `RecvBitrate`：音频接收码率（Kbps）。
  - `RecvLevel`：接收音频的音量。
  - `TransportDelay`：传输延时（ms），从远端发送音频到本地接收音频的延时。

``` javascript
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

- [`Client.getRemoteVideoStats`](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#getremotevideostats) 方法获取远端订阅流的**视频**统计数据，一个 uid 对应一组数据，包括：
  - `End2EndDelay`：端到端延时（ms），从远端采集视频到本地播放视频的延时。
  - `MuteState`：视频画面是否开启。
  - `PacketLossRate`：远端视频的丢包率（%）。
  - `RecvBitrate`：视频接收码率（Kbps）。
  - `RecvResolutionHeight`：视频接收分辨率高度，单位为像素。
  - `RecvResolutionWidth`：视频接收分辨率宽度，单位为像素。
  - `RenderFrameRate`：渲染帧率（fps），视频解码输出帧率。 
  - `TransportDelay`：传输延时（ms），从远端发送视频到本地接收视频的延时。

``` javascript
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

> `Client.getRemoteAudioStats` 和 `Client.getRemoteVideoStats` 这两个方法需要在 `stream-subscribed` 事件后进行统计，可能耗费 0-3 秒时间返回。
