
---
title: Switch the Client Role
description: 
platform: Web
updatedAt: Fri Nov 02 2018 16:31:19 GMT+0000 (UTC)
---
# Switch the Client Role
On the Web Client, the client role is switched with the following three interfaces:

- `client.publish`: Publish the local stream

	```
		client.publish(localStream, function (err) {
				console.log("Publish local stream error: " + err);
		});

		client.on('stream-published', function (evt) {
				console.log("Publish local stream successfully");
		});

	```

- `client.unpublish`: Unpublish the local stream

	```
		client.unpublish(localStream, function (err) {
				console.log("Unpublish local stream error: " + err);
		});

		client.on('stream-unpublished', function (evt) {
				console.log("Unpublish local stream successfully");
		});

	```

- `client.subscribe`: Subscribe to the remote stream

	```
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
	
How to switch the client role:
- Set the user as the host (broadcaster)

  * `client.publish`
  * `client.subscribe`

- Switch the client role from host to audience

  * `client.unpublish`

- Set the user as the audience

  * `client.subscribe`

- Swtich the client role from audience to host

  * `client.publish`

> Create and intialize the stream before calling `client.publish` to publish the local audio and video stream. See [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/.publish_web.md).
