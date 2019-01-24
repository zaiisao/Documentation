
---
title: Create and Initialize a Client
description: 
platform: Web
updatedAt: Thu Jan 24 2019 03:24:40 GMT+0000 (UTC)
---
# Create and Initialize a Client
Before creating and initizing the client, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/web_prepare.md).

## Implementation

### Create a Client
Use the `AgoraRTC.createClient` method to create a client object. Set the mode and codec parameters. 

```javascript
var client = AgoraRTC.createClient({mode: 'live', codec: "h264"});
```

### Initialize the Client
After creating the client, pass the project App ID to the `client.init` method to initialize the client.

```javascript
client.init(<APPID>, function () {
  console.log("AgoraRTC client initialized");

}, function (err) {
  console.log("AgoraRTC client init failed", err);
});
```

## Next Steps
You hava created the Client and can start a voice call with the following steps:
- [Join a Channel](../../en/Video/join_video_web.md)
- [Publish and Subscribe to Streams](../../en/Video/publish_web.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections.
- [Set the Stereo/High-fidelity Audio Profile](../../en/Video/audio_profile_web.md)
