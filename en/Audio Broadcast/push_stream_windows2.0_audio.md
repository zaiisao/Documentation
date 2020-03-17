
---
title: Push Streams to the CDN
description: 
platform: Windows
updatedAt: Fri Sep 27 2019 04:15:53 GMT+0800 (CST)
---
# Push Streams to the CDN
## Introduction

CDN live streaming refers to the process of publishing streams to the CDN (Content Delivery Network), where users can view a live broadcast with a web browser.

You can use [transcoding](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#transcoding) to combine the streams of all hosts in the channel into one stream, or set the audio/video profiles and picture-in-picture layout for the stream to be published to the CDN.

The following figure shows a typical CDN-pushing scenario.

![](https://web-cdn.agora.io/docs-files/1550737160039)


## Prerequisites

Ensure that you enable the RTMP Converter service before using this function.

1. Log in [Console](https://dashboard.agora.io/), and click ![img](https://web-cdn.agora.io/docs-files/1551260936285) in the left navigation menu to go to the **Products & Usage** page. 
2. Select a project from the drop-down list in the upper-left corner, and click **Duration** under **RTMP Converter**. 
![](https://web-cdn.agora.io/docs-files/1569302661254)
3. Click **Enable RTMP Converter**.
4. Click **Apply** to enable the RTMP Converter service and get 500 max concurrent channels.

<div class="alert note"> The number of concurrent channels, N, means that users can push streams to the CDN with transcoding in N channels of media streams. </div>

Now, you can use the function and see the usage statistics.



## Implementation

Agora's CDN publishing solution is based on the following API methods to publish streams to the CDN, inject external audio streams, transcode, and set the output layout.

-   [`setLiveTranscoding`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde): Sets the live transcoing configuration
-   [`addPublishStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839): Adds a stream to the CDN
-   [`removePublishStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7): Removes a stream from the CDN

In which:

-  The host calls the `setLiveTranscoding` method to set the transcoding parameters, for example, the canvas settings, after joining a channel. The host still needs to set a 16 &times; 16 view when only publishing an audio stream to CDN.
-  The host adds or removes a URL with the `addPublishStreamUrl` and `removePublishStreamUrl` methods after joining the channel.


### Sample Code

```C++
// CDN transcoding settings.
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
config.audioBitrate = 48;
config.width = 16;
config.height = 16;
config.videoFramerate = 15;
config.videoCodecProfile = HIGH;

// Sets the output layout for each user.
config.transcodingUser = new TranscodingUser[config.userCount];
config.transcodingUser[0].uid = 123456; // The uid must be identical to the uid used in joinChannel().
config.transcodingUser[0].x = 0;
config.transcodingUser[0].audioChannel = 0;
config.transcodingUser[0].y = 0;
config.transcodingUser[0].width = 16;
config.transcodingUser[0].height = 16;

m_rtcEngine->setLiveTranscoding(config);
```

```C++
// Adds a URL to which the host pushes a stream.
// Set the transcodingEnabled parameter as true to enable the transcoding service. Once transcoding is enabled, you nee to set the live transcoding configurations by calling the setLiveTranscoding method. We do not recommend transcoding in the case of a single host.
m_rtcEngine->addPublishStreamUrl(url, true);

 if (config.transcodingUsers)
   {
      delete[] config.transcodingUsers;
   }
```


```C++
// Removes a URL to which the host pushes a stream.
m_rtcEngine->removePublishStreamUrl(url);
```




