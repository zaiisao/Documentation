
---
title: Inject an Online Media Stream
description: 
platform: Android
updatedAt: Fri Nov 02 2018 17:11:02 GMT+0000 (UTC)
---
# Inject an Online Media Stream
## Introduction

**Injecting online media stream** refers to injecting an external audio or video stream to an ongoing live broadcast channel, so that the host(s) and audience in the channel can hear and see the stream while interacting with each other. 

As of v2.1.0, Agora SDK provides the `addInjectStreamUrl` interface, with which:

- The host specifies a media stream as the input source, injects it into the channel, and pushes it to everyone in the audience.
- The host can set the video profile of the injected video stream.
- If the host enables CDN streaming, the injected media stream is also pushed to all CDN audience.

## Applicable Scenarios

Online media stream injection can be applied to the following scenarios:

- During game events, by injecting the video stream of an ongoing game, the host(s) and audience can enjoy watching the game while making comment on it.
- The same applies to music shows, movies and entertainment shows. Both the host(s) and audience can have on-time discussions and exchange ideas  while watching the show.
- Video streams captured by drones or network cameras can be injected into a live broadcast and broadcasted to all the audience in the channel.

## Considerations

- Only one online media stream can be injected into the same channel at the same time.
- Only the host(broadcaster) can inject and remove an injected media stream. Neither the delegated host nor the audience can do that.
- To inject a media stream, the host need to be in the channel. To receive the injected media stream, the audience need to subscribe to the host.
- Supported media stream formats include: RTMP, HLS and FLV. Audio-only streams can also be injected.
- If the media stream is injected successfully, the media stream will appear in the channel, and the `onUserJoined` and `onFirstRemoteVideoDecoded` callbacks will be triggered, in which the `uid` is 666.
- If the media stream is not injected successfully, the SDK may return the following error codes:

  - `ERR_INVALID_ARGUMENT(2)`: The injected URL does not exist. Call this method again to inject the stream and make sure that the URL is valid.
  - `ERR_NOT_INITIALIZED(7)`: The SDK is not initialized. Make sure that the `RtcEngine` object is initialized before using this method.
  - `ERR_NOT_SUPPORTED(4)`: The channel profile is not live broadcast. Call `setChannelProfile` and set the channel to the live broadcast profile before calling this method.
  - `ERR_NOT_READY(3)`: The App is not in the channel. Make sure that the App has joined the channel.


## Implementations

To inject an online media stream, the user need first join a live broadcast channel in the "broadcaster" role. For how to intialize the engine and join a live broadcast channel, see [Quickstart Guide](https://docs.agora.io/en/Interactive%20Broadcast/android_video?platform=Android).

- To inject an online media stream:

	The broadcaster(host) in the live broadcast channel can call `addInjectStreamUrl` to specify an online media stream and inject it into the channel.

	```java
	//Java
	String urlPath = "Some online RTMP/HLS url path"

	LiveInjectStreamConfig config = new LiveInjectStreamConfig();
	config.width = 0;
	config.height = 0;
	config.videoGop = 30;
	config.videoFramerate = 15;
	config.videoBitrate = 400;
	config.audioSampleRate = LiveInjectStreamConfig.AudioSampleRateType.TYPE_44100;        config.audioBitrate = 48;
	config.audioChannels = 1;

	rtcEngine.addInjectStreamUrl(urlPath, config);
	```

	You can modify the parameter values of `config` to set the resolution, bitrate, frame rate and audio sampling rate of the injected stream. For more information, see [Agora Live Inject Stream Config](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_inject_stream_config.html).
	
- To remove an injected media stream:

	The broadcaster(host) in the live broadcast channel can call `removeInjectStreamUrl` to remove a previously injected media stream.

	```java
	//Java
	String urlPath = "The same online RTMP/HLS url path added before"
	rtcEngine.removeInjectStreamUrl(urlPath)
	```

	> If the host has left the channel, it is unnecessary to call `removeInjectStreamUrl`.

## Working Principles

- The host in a live broadcast channel pulles an online media stream, pushes it to Agora SD-RTN, and eventually into the live broadcast channel via the Video Inject Server. Both the host and the audience in the channel can hear/see the media stream.
- If the host has enabled CDN streaming, the injected media stream is also pushed to CDN so that all the CDN audience can hear/see the media stream.
