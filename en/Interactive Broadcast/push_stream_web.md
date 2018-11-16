
---
title: Push Streams to the CDN
description: 
platform: Web
updatedAt: Fri Nov 16 2018 05:56:29 GMT+0000 (UTC)
---
# Push Streams to the CDN
## Introduction

The CDN live streaming feature enables a host (broadcaster) to transform his or her uplink stream into RTMP and distribute it through different channels such as Web browser or streaming media player. 

> Contact sales@agora.io to enable Agora's CDN live streaming feature. 

On the website, you can push streams by following the steps in the following figure:

<img alt="../_images/push_stream_web.png" src="https://web-cdn.agora.io/docs-files/en/push_stream_web.png" style="width: 420px;"/>

> Contact [sales@agora.io](mailto:sales@agora.io) to enable this function.

## Implementation

### 1. Check Browser Compatibility

```javascript
checkSystemRequirements()
```

### 2. Create A Client Object

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

### 6. Start a Live Stream

```javascript
client.setLiveTranscoding(coding);
//if enableTranscoding is set to true, setLiveTranscoding must be called before _startLiveStreaming
client.startLiveStreaming(url, true)
```

### 7. Set Live Transcoding

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

### 8. Stop Live Streaming

```javascript
client.stopLiveStreaming(url);
```

### 9. Leave the Channel

```javascript
leave(onSuccess, onFailure)
```
