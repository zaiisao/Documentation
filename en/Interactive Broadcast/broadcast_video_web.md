
---
title: Starting a Live Video Broadcast
description: 
platform: Web
updatedAt: Wed Feb 13 2019 07:08:28 GMT+0000 (UTC)
---
# Starting a Live Video Broadcast
This page introduces how to use Agora’s Web SDK to start a live video broadcast.

This quickstart uses Agora’s [sample app](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-Web-Tutorial-1to1) as an example.

See the **README** file in the sample app for details on how the sample app works.

## Prerequisites

See [Setting up Your Development Environment](../../en/Interactive%20Broadcast/web_prepare.md) for the the prerequisites, environment setup, and SDK integration guide.

## Using Core APIs to Start a Live Video Broadcast

The following lists the major steps to start a live video broadcast on the Web：

1. [Create a Client](#createClient)
2. [Initialize the Client](#initClient)
3. [Join a Channel](#join)
4. [Create a Stream](#createStream)
5. [Initialize the Stream](#initStream)
6. [Publish the Local Stream](#pub)
7. [Subscribe a Remote Stream](#sub)
8. [Play the Stream](#play)
9. [Leave the Channel](#leave)

See the image below for the key steps in the process:

![](https://web-cdn.agora.io/docs-files/1550041581805)

The `index.html` file in the sample app contains the complete code of the following APIs for the reference. Use these APIs in a web app to start a live video broadcast.

### <a name = "createClient"></a>Create a Client

Use the `AgoraRTC.createClient` method to create a client object. Set the mode and codec parameters. 

```javascript
var client = AgoraRTC.createClient({mode: 'live', codec: "h264"});
```

> The broadcast client object can only be created once per call session.

### <a name = "initClient"></a>Initialize the Client

After creating the client, pass the project App ID to the `client.init` method to initialize the client.

```javascript
client.init(<APPID>, function () {
  console.log("AgoraRTC client initialized");

}, function (err) {
  console.log("AgoraRTC client init failed", err);
});
```

### <a name = "join"></a>Join a Channel

Once the client initialization is complete, call the  `client.join`  method in the `onSuccess` callback.

Pass the channel key, channel name, and user ID to the method parameters:

- **DYNAMIC\_KEY**: For low-security requirements, pass null as the parameter value.
- **CHANNEL\_NAME**: Channel name.
- **UID**: The user ID is an integer and should be unique. If you set the user ID to null, the Agora server assigns a user ID and returns it in the onSuccess callback.

```javascript
client.join(<DYNAMIC_KEY>, <CHANNEL_NAME>, <UID>, function(uid) {
  console.log("User " + uid + " join channel successfully");

}, function(err) {
  console.log("Join channel failed", err);
});
```

> Users with different App IDs cannot call each other even if they join the same channel.

### <a name = "createStream"></a>Create a Stream

Use the `client.createStream`  method to create a stream.

The sample app passes in an object with the following properties:

- **streamID**: The stream ID, set as the user ID, which can be retrieved from the callback of `client.join` .
- **audio**: Enable/Disable audio.
- **video**: Enable/Disable video.
- **screen**: Enable/Disable screen sharing. Do not set `video` and `screen` as `true` at the same time.

```javascript
localStream = AgoraRTC.createStream({
  streamID: uid,
  audio: true,
  video: true,
  screen: false}
);
```

The `createStream` object has additional optional properties. 

### <a name = "initStream"></a>Initialize the Stream

Next, call the `stream.init`  method to initialize the stream.

```javascript
localStream.init(function() {
  console.log("getUserMedia successfully");
  localStream.play('agora_local');

}, function (err) {
  console.log("getUserMedia failed", err);
});
```

### <a name = "pub"></a>Publish the Local Stream

Once initialized, use the `client.publish` method in the onSuccess callback to publish the stream.

```javascript
client.publish(localStream, function (err) {
  console.log("Publish local stream error: " + err);
});

client.on('stream-published', function (evt) {
  console.log("Publish local stream successfully");
});
```

### <a name = "sub"></a>Subscribe a Remote Stream

To subscribe a remote stream:

1. Use the `client.on('stream-added')` event listener to detect when a new stream is added to the channel.
2. When the event is detected, use the `client.subscribe`  method in the callback to subscribe the stream.

```javascript
client.on('stream-added', function (evt) {
  var stream = evt.stream;
  console.log("New stream added: " + stream.getId());

  client.subscribe(stream, function (err) {
    console.log("Subscribe stream failed", err);
  });
});
client.on('stream-subscribed', function (evt) {
  var remoteStream = evt.stream;
  console.log("Subscribe remote stream successfully: " + stream.getId());
  stream.play('agora_remote' + stream.getId());
})
```

### <a name = "play"></a>Play the Stream

After initializing the local stream or subscribing the remote stream, use the `stream.play`  method to play the stream on the web page. The `stream.play`  method takes the ID of a dom element as paramter, and SDK creates a `<video`\> tag and plays the video.

#### Play the stream after initializing the local stream

```javascript
localStream.init(function() {
    console.log("getUserMedia successfully");
    // Use agora_local as ID of the dom element
    localStream.play('agora_local');

  }, function (err) {
    console.log("getUserMedia failed", err);
  });
```

#### Play the stream after subscribing the remote stream

```javascript
client.on('stream-subscribed', function (evt) {
  var remoteStream = evt.stream;
  console.log("Subscribe remote stream successfully: " + stream.getId());
  // Use agora_remote + stream.getId() as the ID of the dom element
  remoteStream.play('agora_remote' + stream.getId());
})
```

### <a name = "leave"></a>Leave the Channel

Use the `client.leave`  method to remove the user from the current broadcast \(channel\).

```javascript
client.leave(function () {
  console.log("Leave channel successfully");
}, function (err) {
  console.log("Leave channel failed");
});
```

## Complete Code

For the complete code, see the `index.html` file in the sample app.

Now you can run the app, enter a channel name, and start a live video broadcast.

> Ensure your camera and microphone work before starting a live video broadcast.

## References

The following list are references to the Agora APIs used on this page:

- Create a Client Object: `AgoraRTC.createClient`
- Initialize a Client Object: `client.init`
- Join an AgoraRTC Channel: `client.join`
- Create a Stream Object: `client.createStream`
- Initialize the Stream Object: `stream.init`
- Publish a local stream: `client.publish`
- Subscribe to a Remote Stream: `client.subscribe`
- Play the Video/Audio Stream: `stream.play`
- Leave an AgoraRTC channel: `client.leave`

Refer to the following links to add more functions to a live video broadcast:

- [Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy_web.md)
- [Screen Sharing on the Web](../../en/Quickstart%20Guide/screensharing_web.md)

Find all the Agora APIs that can be implemented in a live video broadcast in [Agora Web SDK API](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/index.html).
