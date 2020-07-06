
---
title: Inject Online Media Stream
description: 
platform: Android
updatedAt: Mon Jul 06 2020 10:45:07 GMT+0800 (CST)
---
# Inject Online Media Stream
## Introduction

**Injecting an online media stream** is the action of adding an external audio or video stream to an ongoing live interactive streaming channel. It enables the hosts and audience in the channel to hear and see the additional stream while interacting with each other.

### Applicable scenarios

- Live sports: The host and audience can watch and simultaneously comment on events.
- Music concerts, movies, and other entertainments: The hosts and audience can participate in real‑time discussions while watching them.
- Additional perspectives: The host can inject video streams captured by drones or network cameras into a live streaming channel.

### Working principles

The host in a live interactive streaming channel pulls an online media stream and pushes it through the Video Inject Server to the Agora Software‑Defined Real‑time Network (SD‑RTN™) and the channel.

![](https://web-cdn.agora.io/docs-files/1576059890625)

- The host and audience in the channel can hear and see the media stream.
- If the host enables Content Delivery Network (CDN) live streaming, the injected media stream is also pushed to the CDN so that the CDN audience can hear and see the media stream.

>- Only one online media stream can be injected into the same channel at the same time.
>- Supported codec type: AAC for audio, H.264 for video.
>- Audio-only streams are also supported.
>- Only the host can inject and remove an injected media stream. Neither the delegated host nor the audience can do that.


## Implementation

Before proceeding, ensure that you implement the basic live interactive streaming in your project. See [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_android.md) for details.

> Ensure that you enable the RTMP Converter service before using this function. See [Prerequisites](../../en/Interactive%20Broadcast/cdn_streaming_android.md).

Refer to the following steps to inject an online media stream:

1. The host in a channel calls the `addInjectStreamUrl` method to inject an online media stream to the live interactive streaming channel. You can modify the parameter values of `config` to set the resolution, bitrate and frame rate of the injected stream. See [InjectStreamConfig](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_inject_stream_config.html).
	> Only one online media stream can be injected into the same channel at the same time.

	If the method call is successful, SDK triggers the `onUserJoined (uid:666)` callback to all the users in the channel, and triggers the `onStreamInjectedStatus` callback to the local host.
	> The local host can troubleshoot with [API Reference](#api) when exceptions occur.
	
2. The host in a channel calls the `removeInjectStreamUrl` method to remove the injected media stream.
	If the method call is successful, SDK triggers the `onUserOffline (uid:666)` callback to all the users in the channel.
	> Do not need to call the `removeInjectStreamUrl` method if the host has left the channel.


### Sample code

```java
// Java
// Inject an online media stream.
String urlPath = "Some online RTMP/HLS url path"

  LiveInjectStreamConfig config = new LiveInjectStreamConfig();
  config.width = 0;
  config.height = 0;
  config.videoGop = 30;
  config.videoFramerate = 15;
  config.videoBitrate = 400;
  config.audioSampleRate = LiveInjectStreamConfig.AudioSampleRateType.TYPE_48000; 
  config.audioBitrate = 48;
  config.audioChannels = 1;

  rtcEngine.addInjectStreamUrl(urlPath, config);

// Remove an online media stream.
String urlPath = "The same online RTMP/HLS url path added before"
  rtcEngine.removeInjectStreamUrl(urlPath)
```

We also provide an open-source [Live-Streaming-Injection](https://github.com/AgoraIO/Advanced-Interactive-Broadcasting/tree/master/Live-Streaming-Injection) demo project on GitHub.

<a name="api"></a>
### API reference

- [`addInjectStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a67547508dd8b98318b55d764eb0da311)
- [`removeInjectStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a1bd152dba2c28459ff5202bb8039fb42)
- [`onStreamInjectedStatus`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a470bb5a47f90705fa3da3e3b6aebb28d)
- [`onUserJoined`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa466d599b13768248ac5febd2978c2d3)
- [`onUserOffline`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9fbb08177fbc8f74d64044a78aea0dda)

## Considerations
To receive the injected media stream, the audience need to subscribe to the host.
