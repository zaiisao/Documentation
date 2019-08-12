
---
title: Push Streams to the CDN
description: 
platform: Web
updatedAt: Mon Aug 12 2019 10:23:39 GMT+0800 (CST)
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
  width: 640,
  height: 360,
  videoBitrate: 400,
  videoFramerate: 15,
  lowLatency: false,
  audioSampleRate: 48000,
  audioBitrate: 48,
  audioChannels: 1,
  videoGop: 30,
  videoCodecProfile: 100,
  userCount: 0,
  userConfigExtraInfo: {},
  backgroundColor: 0x000000,
  transcodingUsers: [],
};
```

### 7. Start a Live Stream

```javascript
client.setLiveTranscoding(coding);
//If enableTranscoding is set to true, setLiveTranscoding must be called before _startLiveStreaming.
client.startLiveStreaming(url, true)
```

### 8. Stop Live Streaming

```javascript
client.stopLiveStreaming(url);
```

### 9. Leave the Channel

```javascript
leave(onSuccess, onFailure)
```
