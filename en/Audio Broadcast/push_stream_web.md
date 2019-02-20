
---
title: Push Streams to the CDN
description: 
platform: Web
updatedAt: Mon Dec 10 2018 21:40:31 GMT+0000 (UTC)
---
# Push Streams to the CDN
## Introduction

The CDN live streaming feature enables a host (broadcaster) to transform the uplink stream into RTMP and distribute it through different channels such as the Web browser or streaming media player. 

> Contact sales@agora.io to enable Agora's CDN live streaming feature. 

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
