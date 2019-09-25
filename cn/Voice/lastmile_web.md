
---
title: 通话前的网络质量检测
description: 通话前的网络质量检测
platform: Web
updatedAt: Wed Jul 17 2019 09:46:17 GMT+0800 (CST)
---
# 通话前的网络质量检测
## 功能描述

在加入频道或切换角色为主播前，进行网络质量探测，可以判断或预测用户当前的网络状况是否良好，可以满足音频码率或者当前选定的视频属性的目标码率。

在对网络质量要求高的场景下，Agora 建议在加入频道前进行探测，保证通信顺畅。

## 实现方法

```javascript
// testing WebRTC network full roundtrip requires two clients to
// simulate stream publish/subscribe
// ---------------------------------
// |	    local   <----   remote   |
// |              subscribe         |
// ---------------------------------
//
// 1. remote client that push streams only
var remoteClient = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
// ... init client and join
var remoteStream = AgoraRTC.createStream({
	streamID: remoteUid,
	audio: true,
	video: true,
	screen: false
});
// ... init stream
remoteStream.publish();
		
// 2. local client that subscribe remote stream
var localClient = AgoraRTC.createClient({ mode: 'live', codec:'h264' });
// ... init client and join

// 3. start timer getting network stats
setInterval(function(){
	localClient.getRemoteVideoStats(function(statsMap){
		// video stats map for remote streams, indexed by uid
		var stats = statsMap[remoteUid];
		console.log(JSON.stringify(stats));
	})
		
	localClient.getRemoteAudioStats(function(statsMap){
		// audio stats map for remote streams, indexed by uid
		var stats = statsMap[remoteUid];
		console.log(JSON.stringify(stats));
	})
		
	localClient.getTransportStats(function(stats){
		// gateway network stats
		console.log(JSON.stringify(stats));
	})
}, 2 * 1000);
```

### API 参考

- [`getRemoteVideoStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremotevideostats)
- [`getRemoteAudioStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremoteaudiostats)
- [`getTransportStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#gettransportstats)

## 开发注意事项

- 远端视频/音频流信息获取仅在加入频道后且订阅对应流的状态下有效
