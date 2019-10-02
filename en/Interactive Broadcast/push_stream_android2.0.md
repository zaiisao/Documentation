
---
title: Push Streams to the CDN
description: 
platform: Android
updatedAt: Fri Sep 27 2019 04:10:34 GMT+0800 (CST)
---
# Push Streams to the CDN
## Introduction

CDN live streaming refers to the process of publishing streams to the CDN (Content Delivery Network), where users can view a live broadcast with a web browser.

You can use [transcoding](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#transcoding) to combine the streams of all hosts in the channel into one stream, or set the audio/video profiles and picture-in-picture layout for the stream to be published to the CDN.

The following figure shows a typical CDN-pushing scenario.

<img alt="../_images/live_ios_publishing_stream_en.png" src="https://web-cdn.agora.io/docs-files/en/live_ios_publishing_stream_en.png"/>

## Prerequisites

Ensure that you enable the RTMP Converter service before using this function.

1. Log in [Dashboard](https://dashboard.agora.io/), and click ![img](https://web-cdn.agora.io/docs-files/1551260936285) in the left navigation menu to go to the **Products & Usage** page. 
2. Select a project from the drop-down list in the upper-left corner, and click **Duration** under **RTMP Converter**. 
![](https://web-cdn.agora.io/docs-files/1569302661254)
3. Click **Enable RTMP Converter**.
4. Click **Apply** to enable the RTMP Converter service and get 500 max concurrent channels.

<div class="alert note"> The number of concurrent channels, N, means that users can push streams to the CDN with transcoding in N channels of media streams. </div>

Now, you can use the function and see the usage statistics.


## Implementation

Agora's CDN publishing solution is based on the following API methods to publish streams to the CDN, inject external video streams, transcode, and set the output layout.

-   [`setLiveTranscoding`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a3cb9804ae71819038022d7575834b88c): Sets the live transcoding configuration
-   [`addPublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4445b4ca9509cc4e2966b6d308a8f08f): Adds a stream to the CDN
-   [`removePublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a87b3f2f17bce8f4cc42b3ee6312d30d4): Removes a stream from the CDN

In which:

-  The host calls the `setLiveTranscoding` method to set the transcoding parameters, for example, the canvas settings, after joining a channel. The host still needs to set a 16 &times; 16 view when only publishing an audio stream to CDN.
-  The host adds or removes a URL with the `addPublishStreamUrl` and `removePublishStreamUrl` methods after joining the channel.

### Sample Code

```java
// CDN transcoding settings.
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
config.audioBitrate = 48;
config.width = 640;
config.height = 720;
config.videoFramerate = 30;
config.userCount = 1;  // If userCount > 1, set layout for each user.
config.videoCodecProfile = HIGH;

// Sets the output layout for each user.
LiveTranscoding transcoding = new LiveTranscoding();
LiveTranscoding.TranscodingUser user = new LiveTranscoding.TranscodingUser();
user.uid = 123456;
transcoding.addUser(user);
user.x = 0;
user.audioChannel = 0;
user.y = 0;
user.width = 640;
user.height = 720;

rtcEngine.setLiveTranscoding(transcoding);
```

```java
// Adds a URL to which the host pushes a stream.
// Set the transcodingEnabled parameter as true to enable the transcoding service. Once transcoding is enabled, you nee to set the live transcoding configurations by calling the setLiveTranscoding method. We do not recommend transcoding in the case of a single host.
rtcEngine.addPublishStreamUrl(url, true);
```

```java
// Removes a URL to which the host pushes a stream.
rtcEngine.removePublishStreamUrl(url);
```

### Adjusting the Picture-in-picture Layout

Use transcodingUser to adjust the picture-in-picture layout when a channel has multiple hosts.

> A host can set transcoding and the layout, for example, the canvas settings and multiple-host window settings, only after joining a channel.

**1: Two-host Tile Horizontally**

To display the following layout:

<img alt="../_images/sei_2host.png" src="https://web-cdn.agora.io/docs-files/en/sei_2host.png" style="width: 420px;"/>

Set the parameters as follows:

```
Canvas:
     width: 640
     height: 360
     backgroundColor: #FFFFFF

User0:
      userId: 123
      x: 0
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0

User1:
      userId: 456
      x: 320
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0
```

**Example 2: Three-host Tile Vertically**

To display the following layout:

<img alt="../_images/sei_3host.png" src="https://web-cdn.agora.io/docs-files/en/sei_3host.png" style="width: 420px;"/>

Set the parameters as follows:

```
Canvas:
      width: 360
      height: 640
      backgroundColor: #FFFFFF

   User0:
      userId: 123
      x: 0
      y: 0
      width: 360
      height: 640
      zOrder: 1
      alpha: 1.0

   User1:
       userId: 456
       x: 0
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0

   User2:
       userId: 789
       x: 180
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0
```

**Example 3: One Full Screen + Floating Thumbnails**

To display the following layout:

<img alt="../_images/sei_random.png" src="https://web-cdn.agora.io/docs-files/en/sei_random.png" style="width: 840px;"/>

Set the parameters as follows:

```
Canvas:
   width: 360
   height: 640
   backgroundColor: #FFFFFF

User0:
   userId: 123
   x: 0
   y: 0
   width: 360
   height: 640
   zOrder: 1
   alpha: 1.0

User1:
    userId: 456
    x: 45
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0

User2:
    userId: 789
    x: 185
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0
```


