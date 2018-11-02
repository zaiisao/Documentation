
---
title: Publish and Subscribe to Streams
description: 
platform: Web
updatedAt: Thu Nov 01 2018 09:06:41 GMT+0000 (UTC)
---
# Publish and Subscribe to Streams
# Publish and Subscribe to Streams
## Create a stream
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

## Initialize the stream
Next, call the `stream.init`  method to initialize the stream.

```javascript
localStream.init(function() {
  console.log("getUserMedia successfully");
  localStream.play('agora_local');

}, function (err) {
  console.log("getUserMedia failed", err);
});
```

## Publish the local stream
Once initialized, use the `client.publish` method in the `onSuccess` callback to publish the stream.

```javascript
client.publish(localStream, function (err) {
  console.log("Publish local stream error: " + err);
});

client.on('stream-published', function (evt) {
  console.log("Publish local stream successfully");
});
```

## Subscribe to the remote stream
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

## Play the stream
After initializing the local stream or subscribing the remote stream, use the `stream.play`  method to play the stream on the web page. The `stream.play`  method takes the ID of a dom element as paramter, and SDK creates a `<video>` tag and plays the audio.

- Play the stream after initializing the local stream

	```javascript
	localStream.init(function() {
			console.log("getUserMedia successfully");
			// Use agora_local as ID of the dom element
			localStream.play('agora_local');

		}, function (err) {
			console.log("getUserMedia failed", err);
		});
	```

- Play the stream after subscribing the remote stream

	```javascript
	client.on('stream-subscribed', function (evt) {
		var remoteStream = evt.stream;
		console.log("Subscribe remote stream successfully: " + stream.getId());
		// Use agora_remote + stream.getId() as the ID of the dom element
		remoteStream.play('agora_remote' + stream.getId());
	})
	```

