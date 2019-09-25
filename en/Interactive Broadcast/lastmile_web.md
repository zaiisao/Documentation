
---
title: Conduct a Last-mile Test
description: 
platform: Web
updatedAt: Wed Jul 17 2019 09:46:36 GMT+0800 (CST)
---
# Conduct a Last-mile Test
## Introduction

To check if the uplink network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before joining the channel.

This function particularly applies to scenarios which have high requirements on the network quality.



## Implementation

```javascript
// Testing the WebRTC network round trip requires two clients to
// simulate stream publish/subscribe.
// ---------------------------------
// |	    local   <----   remote   |
// |              subscribe         |
// ---------------------------------
//
// 1. Remote client that pushes streams only.
var remoteClient = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
// Initialize the client and join the channel.
var remoteStream = AgoraRTC.createStream({
	streamID: remoteUid,
	audio: true,
	video: true,
	screen: false
});
// Initialize the stream.
remoteStream.publish();
		
// 2. Local client that subscribes to the remote stream.
var localClient = AgoraRTC.createClient({ mode: 'live', codec:'h264' });
// Initialize the client and join the channel.

// 3. Start the timer getting the network statistics.
setInterval(function(){
	localClient.getRemoteVideoStats(function(statsMap){
		// Video statistics map for the remote streams, indexed by uid.
		var stats = statsMap[remoteUid];
		console.log(JSON.stringify(stats));
	})
		
	localClient.getRemoteAudioStats(function(statsMap){
		// Audio statistics map for the remote streams, indexed by uid.
		var stats = statsMap[remoteUid];
		console.log(JSON.stringify(stats));
	})
		
	localClient.getTransportStats(function(stats){
		// Gateway network statistics.
		console.log(JSON.stringify(stats));
	})
}, 2 * 1000);
```

### API Reference

- [`getRemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremotevideostats)
- [`getRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremoteaudiostats)
- [`getTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#gettransportstats)

## Considerations

- The acquired remote audio/video stream information is valid only after joining a channel and after subscribing to the corresponding stream. 
