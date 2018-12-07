
---
title: Conduct a Last Mile Test
description: 
platform: Web
updatedAt: Wed Dec 05 2018 03:12:23 GMT+0000 (UTC)
---
# Conduct a Last Mile Test
## Introduction

You can conduct a last mile network quality test before a call to check if the network supports the audio bitrate or target bitrate of the chosen video profile before a user joins a channel. The `onLastmileQuality` callback reports the test results once every two seconds. The test results are based on the network quality ratings determined by the packet-loss rates and network jitter, which reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 Kbps. 
> The video SDK adjusts the actual bitrate according to the chosen video profile.



## Implementation

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

## API Reference

- [getRemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremotevideostats)
- [getRemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremoteaudiostats)
- [getTransportStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#gettransportstats)

## Considerations

- Only after joining a channel and after subscribing to the corresponding stream is the acquired remote video/audio stream information valid. 
