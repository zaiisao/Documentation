
---
title: Conduct a Last Mile Test
description: 
platform: Web
updatedAt: Fri Dec 07 2018 16:35:04 GMT+0000 (UTC)
---
# Conduct a Last Mile Test
## Introduction

You can conduct a last mile network quality test before a call to check if the network supports the audio bitrate or target bitrate of the chosen video profile before a user joins a channel. The `onLastmileQuality` callback reports the test results once every two seconds. The test results are based on the network quality ratings determined by the packet-loss rates and network jitter, which reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 Kbps. 
> The video SDK adjusts the actual bitrate according to the chosen video profile.



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
// ... init client and join

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

## API Methods

- [getRemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremotevideostats)
- [getRemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getremoteaudiostats)
- [getTransportStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#gettransportstats)

## Considerations

- The acquired remote video/audio stream information is valid only after joining a channel and after subscribing to the corresponding stream. 
