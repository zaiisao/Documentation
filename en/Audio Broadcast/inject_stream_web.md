
---
title: Inject Online Media Stream
description: 
platform: Web
updatedAt: Mon Jul 06 2020 10:35:06 GMT+0800 (CST)
---
# Inject Online Media Stream
## Introduction

**Injecting an online media stream** is the action of adding an external audio or video stream to an ongoing live-broadcast channel. It enables the hosts and audience in the channel to hear and see the additional stream while interacting with each other.


<div class="alert info">Click the <a href="https://webdemo.agora.io/agora-web-showcase/examples/Agora-Interactive-Broadcasting-Live-Streaming-Injection-Web/">online demo</a> to try this feature out.</div>

### Applicable scenarios

- Live sports: The host and audience can watch and simultaneously commenting on events.
- Music concerts, movies, and other entertainments: The hosts and audience can participate in real-time discussions while watching them.
- Additional perspectives: The host can inject video streams captured by drones or network cameras into a live broadcast.

### Working principles

The host in a live-broadcast channel pulls an online media stream and pushes it through the Video Inject Server to the Agora Software-Defined Real-time Network (SD-RTN™) and the channel.

![](https://web-cdn.agora.io/docs-files/1576059890625)

- The host and audience in the channel can hear and see the media stream.
- If the host enables Content Delivery Network (CDN) live streaming, the injected media stream is also pushed to the CDN so that the CDN audience can hear and see the media stream.

<div class="alert note">Note:
	<ul><li>Only one online media stream can be injected into the same channel at the same time.</li>
		<li>Supported codec type: AAC for audio, H.264 for video.</li>
		<li>Audio-only streams are also supported.</li>
		<li>Only the host (broadcaster) can inject and remove an injected media stream. Neither the delegated host nor the audience can do that.</li>
	</ul>
</div>

## Implementation

Before proceeding, ensure that you implement a basic live broadcast in your project. See [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_web.md) for details.

> Ensure that you enable the RTMP Converter service before using this function. See [Prerequisites](../../en/Audio%20Broadcast/cdn_streaming_web.md).

Refer to the following steps to inject an online media stream:

1. The host in a channel calls the `Client.addInjectStreamUrl` method to inject an online media stream to the live broadcast channel. You can modify the parameter values of `config` to set the resolution, bitrate and frame rate of the injected stream. See [InjectStreamConfig](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.injectstreamconfig.html).
	> Only one online media stream can be injected into the same channel at the same time.

	If the method call is successful, SDK triggers the `Client.on("stream-added"` and `Client.on("peer-online")` callbacks to all the users in the channel, and triggers the **`Client.on("streamInjectedStatus")`** callback to the local host.
	> The local host can troubleshoot with [API documentation](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#on) when exceptions occur.
	
2. The host in a channel calls the `Client.removeInjectStreamUrl` method to remove the injected media stream.
	If the method call is successful, SDK triggers the `Client.on("peer-leave")` and `Client.on("stream-removed")`callbacks to all the users in the channel.
	> You do not need to call this method if the host has left the channel.


### Sample code

```javascript
// Javascript
// Inject an online media stream.
var InjectStreamConfig = {
   width: 0,
   height: 0,
   videoGop: 30,
   videoFramerate: 15,
   videoBitrate: 400,
   audioSampleRate: 44100,
   audioChannels: 1,
   };

Client.addInjectStreamUrl(url, config);

// Remove an online media stream.
Client.removeInjectStreamUrl(url);
```

We also provide an open-source [Live-Streaming-Injection](https://github.com/AgoraIO/Advanced-Interactive-Broadcasting/tree/master/Live-Streaming-Injection) demo project on GitHub.

<a name="api"></a>
### API reference

- [`addInjectStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#addinjectstreamurl)
- [`removeInjectStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#removeinjectstreamurl)


## Considerations
To receive the injected media stream, the audience need to subscribe to the host.

## Reference
See also: [When injecting online streams to the CDN, what should I do when a disconnection happens?](https://docs.agora.io/en/faq/injecting_stream_disconnection_web)

