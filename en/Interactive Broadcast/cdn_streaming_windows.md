
---
title: Push Streams to CDN
description: 
platform: Windows
updatedAt: Mon Jul 06 2020 10:35:15 GMT+0800 (CST)
---
# Push Streams to CDN
## Introduction

The process of publishing streams into the CDN (Content Delivery Network) is called CDN live streaming, where users can view the live broadcast through a web browser.

When multiple hosts are in a channel in CDN live streaming, [transcoding](https://docs.agora.io/en/AgoraPlatform/terms?platform=AllPlatforms#transcoding) is used to combine the streams of all the hosts into a single stream. Transcoding sets the audio and/or video profiles and the picture-in-picture layout for the stream to be pushed to the CDN.

![](https://web-cdn.agora.io/docs-files/1588993691567)


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

Before proceeding, ensure that you implement a basic live broadcast in your project. See [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/cdn_streaming_windows.md) for details.

Refer to the following steps to push streams to the CDN:

<a name="single"></a>
1. The host in a channel calls the `setLiveTranscoding` method to set the transcoding parameters of the media streams (`LiveTranscoding`), such as the resolution, bitrate, frame rate, and position of the watermark/background image. If you need a transcoded picture, set the picture-in-picture layout for each user in the `TranscodingUser` class, as described in [Sample Code](#sample).

   > The `onTranscodingUpdated` callback occurs when the `LiveTranscoding` class updates and reports the updated information to the local host.

2. The host in a channel can call the `addPublishStreamUrl` method to add a media stream to the CDN. 

   > Use `transcodingEnabled` to set whether or not to enable transcoding.

3. The host in a channel can call the `removePublishStreamUrl` method to remove a media stream from the CDN live broadcast.


When the state of the media streams pushed to the CDN changes, the SDK triggers the `onRtmpStreamingStateChanged` callback to report the current state of pushing streams to the local host. The local host can troubleshoot with the error code when exceptions occur.



<a name="sample"></a>
### Sample code

```c++
// C++
// CDN transcoding settings.
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
config.audioBitrate = 48;
// Width of the video (px). The default value is 360.
config.width = 360;
// Height of the video (px). The default value is 640.
config.height = 640;
// Bitrate of the video (Kbps). The default value is 400.
cofig.videoBitrate = 400;
// Frame rate of the video (fps). The default value is 15. Agora adjusts all values over 30 to 30.
config.videoFramerate = 15;
// If userCount > 1, set the layout for each user with transcodingUser.
config.userCount = 1; 
// Video codec profile. Choose to set as Baseline (66), Main (77), or High (100). If you set this parameter to other values, Agora adjusts it to the default value of 100.
config.videoCodecProfile = HIGH;

// Sets the output layout for each user.
config.transcodingUser = new TranscodingUser[config.userCount];
// The uid must be identical to the uid used in joinChannel().
config.transcodingUser[0].uid = 123456; 
config.transcodingUser[0].x = 0;
config.transcodingUser[0].audioChannel = 0;
config.transcodingUser[0].y = 0;
config.transcodingUser[0].width = 640;
config.transcodingUser[0].height = 720;

// CDN transcoding settings when using transcoding.
m_rtcEngine->setLiveTranscoding(config);

// Adds a URL to which the host pushes a stream. Set the transcodingEnabled parameter as true to enable the transcoding service. Once transcoding is enabled, you need to set the live transcoding configurations by calling the setLiveTranscoding method. We do not recommend transcoding in the case of a single host.
m_rtcEngine->addPublishStreamUrl(url, true);

 if (config.transcodingUsers)
   {
      delete[] config.transcodingUsers;
   }

// Removes a URL to which the host pushes a stream.
m_rtcEngine->removePublishStreamUrl(url);
```

**Example 1: Two-host tile horizontally**


<img alt="../_images/sei_2host.png" src="https://web-cdn.agora.io/docs-files/en/sei_2host.png" style="width: 416.0px; height: 240.0px;"/>


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

**Example 2: Three-host tile vertically**

<img alt="../_images/sei_3host.png" src="https://web-cdn.agora.io/docs-files/en/sei_3host.png" style="width: 236.0px; height: 416.0px;"/>

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

**Example 3: One full screen + floating thumbnails**

<img alt="../_images/sei_random.png" src="https://web-cdn.agora.io/docs-files/en/sei_random.png" style="width: 828.0px; height: 416.0px;"/>


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

We also provide an open-source [Live-Streaming](https://github.com/AgoraIO/Advanced-Interactive-Broadcasting/tree/master/Live-Streaming) demo project on GitHub.

### API reference 

- [`setLiveTranscoding`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)
- [`onTranscodingUpdated`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0583fb2aecbd4c51bba1af97ce1aa318)
- [`onRtmpStreamingStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d)


## Considerations

- Up to 17 hosts can be supported in the same live broadcast channel.

- In the case of a single host, we do not recommend transcoding. You can skip [Step1](#single), and call the `addPublishStreamUrl` method directly with `transcodingEnabled (false)`.

  > Currently, Agora only supports pushing H.264 (default) streams to the CDN.

- Set the value of `videoBitrate` by referring to [Video Bitrate Table](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#af10ca07d888e2f33b34feb431300da69). If you set a bitrate beyond the proper range, the SDK automatically adjusts it to a value within the range. 

- Agora charges a transcoding usage fee for CDN live streaming.
