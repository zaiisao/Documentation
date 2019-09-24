
---
title: Push Streams to the CDN
description: 
platform: Web
updatedAt: Tue Sep 24 2019 05:35:49 GMT+0800 (CST)
---
# Push Streams to the CDN
## Introduction

The process of publishing streams into the CDN (Content Delivery Network) is called CDN live streaming, where users can view the live broadcast through a web browser.

When multiple hosts are in the channel in the CDN live streaming, [transcoding](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#transcoding) is used to combine the streams of all the hosts into a single stream. Transcoding sets the audio/video profiles and the picture-in-picture layout for the stream to be pushed to the CDN.

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
> Set `videoBitrate` according to the [Video Bitrate Table](https://docs.agora.io/en/Interactive%20Broadcast/en/Video/API%20Reference/web/v2.9.0/interfaces/agorartc.videoencoderconfiguration.html?transId=2.9.0#bitrate).If you set a bitrate beyond the proper range, the SDK automatically adapts it to a value within the range.

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
