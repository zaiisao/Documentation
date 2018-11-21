
---
title: Conduct a Lastmile Test
description: 
platform: Web
updatedAt: Wed Nov 21 2018 10:42:51 GMT+0000 (UTC)
---
# Conduct a Lastmile Test
## Introduction

The pre-call networkd quality test checks if the current network quality suits the audio bitrate or the target bitrate of the chosen video profile before a user joins a channel. Returned to the user by callbacks every two seconds, the test results are a network quality rating based on packet-loss rate and network jitter and reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 kbps; the video SDK adjust the actual bitrate against the chosen video profile.



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
