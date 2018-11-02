
---
title: Create and Initialize a Client
description: 
platform: Web
updatedAt: Fri Nov 02 2018 04:07:34 GMT+0000 (UTC)
---
# Create and Initialize a Client
## Create a Client
Use the `AgoraRTC.createClient` method to create a client object. Set the mode and codec parameters. 

```javascript
var client = AgoraRTC.createClient({mode: 'live', codec: "h264"});
```

## Initialize the Client
After creating the client, pass the project App ID to the `client.init` method to initialize the client.

```javascript
client.init(<APPID>, function () {
  console.log("AgoraRTC client initialized");

}, function (err) {
  console.log("AgoraRTC client init failed", err);
});
```
