
---
title: Push Streams to the CDN
description: 
platform: iOS,macOS
updatedAt: Thu Feb 21 2019 08:28:07 GMT+0000 (UTC)
---
# Push Streams to the CDN
## Introduction

The CDN live streaming feature enables a host (broadcaster) to transform the uplink stream into RTMP and distribute it through different channels such as the Web browser or streaming media player. 

> Contact sales@agora.io to enable Agora's CDN live streaming feature. 

Agora's CDN publishing solution is based on the following API methods to publish streams to the CDN, inject external audio streams, transcode, and set the output layout.

-   [`addPublishStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)
-   [`removePublishStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:)
-   [`setLiveTranscoding`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLiveTranscoding:)

This solution is flexible and allows:

-   Starting or stopping publishing to the CDN.
-   Adding or removing a streaming URL without interrupting the ongoing publishing.
-   Adding extra controls to the ongoing streams.
-   Using callbacks to monitor the status of the publishing.
-   Quick migration from the legacy approach to the new approach.

## Pushing Streams to the CDN

Contact [sales@agora.io](mailto:sales@agora.io) to enable this function.

> You can enable this function in Dashboard in future releases.
> -  A host can dynamically add or remove a URL after joining the channel.
> -  A host can set transcoding and the layout, for example, the canvas settings, only after joining a channel. The host still needs to set a 16 &times; 16 view when only publishing an audio stream to CDN.

The following figure shows a typical CDN-pushing scenario.

![](https://web-cdn.agora.io/docs-files/1550737392049)


### Sample Code

```objective-c
// CDN transcoding settings.
AgoraLiveTranscodingUser *user = [[AgoraLiveTranscodingUser alloc] init];
user.uid = 12345;
user.rect = CGRectMake(0, 0, 16, 16);
user.audioChannel = 0;

AgoraRtcEngineKit *rtcEngine = [AgoraRtcEngineKit sharedEngineWithAppId:@"" delegate:nil];
AgoraLiveTranscoding *transcoding = [[AgoraLiveTranscoding alloc] init];
transcoding.audioSampleRate = AgoraAudioSampleRateType44100;
transcoding.audioChannels = 0;
transcoding.size = CGSizeMake(16, 16);
transcoding.videoFramerate = 15;
transcoding.videoCodecProfile = AgoraVideoCodecProfileTypeHigh;
transcoding.transcodingUsers = @[user];

[rtcEngine setLiveTranscoding:transcoding];
```

```objective-c
// Add a URL to which the host pushes a stream.
// transcodingEnabled: Whether to enable transcoding. Once enabled, use the setLiveTranscoding method to set the transcoding parameters.
[rtcEngine addPublishStreamUrl:streamUrl transcodingEnabled:NO];
```

```objective-c
// Remove a URL to which the host pushes a stream.
[rtcEngine removePublishStreamUrl:streamUrl];
```
