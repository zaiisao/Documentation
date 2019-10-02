
---
title: Push Streams to the CDN
description: 
platform: Web
updatedAt: Fri Sep 27 2019 03:43:39 GMT+0800 (CST)
---
# Push Streams to the CDN
## Introduction

CDN live streaming refers to the process of publishing streams to the CDN (Content Delivery Network), where users can view a live broadcast with a web browser.

You can use [transcoding](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#transcoding) to combine the streams of all hosts in the channel into one stream, or set the audio/video profiles and picture-in-picture layout for the stream to be published to the CDN.

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

### 1. Check Web Browser Compatibility

```javascript
checkSystemRequirements()
```

### 2. Create a Client Object

```javascript
createClient()
```

### 3. Initialize the Client Object

```javascript
init(appId, onSuccess, onFailure)
```

### 4. Join a Channel

```
join(channelKey, channel, uid, onSuccess, onFailure)
```

### 5. Create A Stream Object

```javascript
createStream(spec)
```

### 6. Set Live Transcoding

```javascript
var LiveTranscoding = {
  width: 640, // Width of the video. Positive integer, the default value is 640. The value range is [16, 10000].
  height: 360, // Height of the video. Positive integer, the default value is 360. The value range is [16, 10000].
  videoBitrate: 400, // Bitrate of the CDN live output video stream. Positive integer. The default value is 400 Kbps. 
  videoFramerate: 15, // Frame rate (fps) of the CDN live output video stream. The default value is 15. Agora adjusts all values over 30 to 30.
  lowLatency: false,
  audioSampleRate: 48000,
  audioBitrate: 48,
  audioChannels: 1,
  videoGop: 30,
  videoCodecProfile: 100, // Video codec profile type: 66, 77, 100. If you set this parameter to other values, Agora adjusts it to the default value 100.
  userCount: 0,
  userConfigExtraInfo: {},
  backgroundColor: 0x000000,
  transcodingUsers: [],
};
```
> Set `videoBitrate` according to the [Video Bitrate Table](https://docs.agora.io/en/Audio%20Broadcast/en/Video/API%20Reference/web/v2.9.0/interfaces/agorartc.videoencoderconfiguration.html?transId=2.9.0#bitrate).If you set a bitrate beyond the proper range, the SDK automatically adapts it to a value within the range.

### 7. Start a Live Streaming

```javascript
client.setLiveTranscoding(coding);
client.startLiveStreaming(url, true) //If enableTranscoding is set to true, setLiveTranscoding must be called before _startLiveStreaming.
```

### 8. Stop Live Streaming

```javascript
client.stopLiveStreaming(url);
```

### 9. Leave the Channel

```javascript
leave(onSuccess, onFailure)
```
