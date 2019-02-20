
---
title: Create and Initialize a Client
description: 
platform: Web
updatedAt: Thu Jan 24 2019 05:25:56 GMT+0000 (UTC)
---
# Create and Initialize a Client
Before creating and initializing the client, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md).

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
You have created the AgoraRtcEngine instance and can start a video call with the following steps:
* [Join a Channel](../../en/Interactive%20Broadcast/join_live_web.md)
* [Switch the Client Role](../../en/Interactive%20Broadcast/role_web.md)
* [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_web_live.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections:

* [Set the Stereo/High-fidelity Audio Profile](../../en/Interactive%20Broadcast/audio_profile_web.md)
