
---
title: Push Streams to the CDN
description: 
platform: Windows
updatedAt: Wed Apr 03 2019 03:54:12 GMT+0800 (CST)
---
# Push Streams to the CDN
## Introduction

The CDN live streaming feature enables a host (broadcaster) to transform the uplink stream into RTMP and distribute it through different channels such as the Web browser or streaming media player. 

> Contact sales@agora.io to enable Agora's CDN live streaming feature. 

Agora's CDN publishing solution is based on the following API methods to publish streams to the CDN, inject external audio streams, transcode, and set the output layout.

-   [`addPublishStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)
-   [`removePublishStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)
-   [`setLiveTranscoding`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)

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

![](https://web-cdn.agora.io/docs-files/1550737618781)


### Sample Code

```
// CDN transcoding settings.
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
//config.audioBitrate
config.width = 16;
config.height = 16;
config.videoFramerate = 15;
config.videoCodecProfile = HIGH;

// Set the output layout for each user.
config.transcodingUser = new TranscodingUser[config.userCount];
config.transcodingUser[0].uid = 123456; // The uid must be identical to the uid used in joinChannel().
config.transcodingUser[0].x = 0;
config.transcodingUser[0].audioChannel = 0;
config.transcodingUser[0].y = 0;
config.transcodingUser[0].width = 16;
config.transcodingUser[0].height = 16;

m_rtcEngine->setLiveTranscoding(config);
// Add a URL to which the host pushes a stream.
m_rtcEngine->addPublishStreamUrl(url, true);

 if (config.transcodingUsers)
   {
      delete[] config.transcodingUsers;
   }
```

```
// Add a URL to which the host pushes a stream.
m_rtcEngine->addPublishStreamUrl(url, false);
```

```
// Remove a URL to which the host pushes a stream.
m_rtcEngine->removePublishStreamUrl(url);
```



