
---
title: Inject Online Media Stream
description: 
platform: iOS
updatedAt: Thu Nov 01 2018 09:48:50 GMT+0000 (UTC)
---
# Inject Online Media Stream
# Inject Online Media Stream

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

## Implementations

To inject an online media stream, the user need first join a live broadcast channel in the "broadcaster" role. For how to intialize the engine and join a live broadcast channel, see [Quickstart Guide](https://docs.agora.io/en/Interactive%20Broadcast/ios_video?platform=iOS).

- To inject an online media stream:

  The broadcaster(host) in the live broadcast channel can call `addInjectStreamUrl` to specify an online media stream and inject it into the channel.
	
	```swift
	//Swift
	let urlPath = "Some online RTMP/HLS url path"
	let config = AgoraLiveInjectStreamConfig.default()
	agoraKit.addInjectStreamUrl(urlPath, config: config)
	```

	You can modify the parameter values of `config` to set the resolution, bitrate, frame rate and audio sampling rate of the injected stream. For more information, see [Agora Live Inject Stream Config](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveInjectStreamConfig.html).
	
- To remove an injected media  stream:

	The broadcaster(host) in the live broadcast channel can call `removeInjectStreamUrl` to remove a previously injected media stream.

	```swift
	//Swift
	let urlPath = "Some online RTMP/HLS url path"
	agoraKit.removeInjectStreamUrl(urlPath)
	```

> If the host has left the channel, it is unnecessary to call `removeInjectStreamUrl`.

## Considerations

- Only one online media stream can be injected into the same channel at the same time.
- Only the host(broadcaster) can inject and remove an injected media stream. Neither the delegated host nor the audience can do that.
- To inject a media stream, the host need to be in the channel. To receive the injected media stream, the audience need to subscribe to the host.
- Supported media stream formats include: RTMP, HLS and FLV. Audio-only streams can also be injected.
- If the media stream is injected successfully, the media stream will appear in the channel, and the `didJoinChannel` and `firstRemoteVideoDecodedOfUid` callbacks will be triggered, in which the `uid` is 666.

## Working Principles

- The host in a live broadcast channel pulles an online media stream, pushes it to Agora SD-RTN, and eventually into the live broadcast channel via the Video Inject Server. Both the host and the audience in the channel can hear/see the media stream.
- If the host has enabled CDN streaming, the injected media stream is also pushed to CDN so that all the CDN audience can hear/see the media stream.
