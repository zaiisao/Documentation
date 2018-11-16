
---
title: 通话前的网络质量检测
description: 通话前的网络质量检测
platform: Web
updatedAt: Fri Nov 16 2018 06:20:07 GMT+0000 (UTC)
---
# 通话前的网络质量检测
## 功能描述

通话前的网络质量检测用于在加入频道前检查用户当前网络状况是否满足当前选定的 video profile 的目标码率。检测结果通过每两秒钟一个回调通知用户, 结果是一个通过丢包率和网络 jitter 计算出来的网络质量打分，主要反映用户的上行网络状况。

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

## 开发注意事项

- 远端视频/音频流信息获取仅在加入频道后且订阅对应流的状态下有效
