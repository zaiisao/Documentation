
---
title: Switch the Client Role
description: 
platform: Web
updatedAt: Mon Jan 28 2019 05:56:44 GMT+0000 (UTC)
---
# Switch the Client Role
Before switching the client role, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md).

## Implementation
On the Web Client, the following methods are used to switch the client role:

- `client.publish`: Publish the local stream.

	```
		client.publish(localStream, function (err) {
				console.log("Publish local stream error: " + err);
		});

		client.on('stream-published', function (evt) {
				console.log("Publish local stream successfully");
		});

	```

- `client.unpublish`: Unpublish the local stream.

	```
		client.unpublish(localStream, function (err) {
				console.log("Unpublish local stream error: " + err);
		});

		client.on('stream-unpublished', function (evt) {
				console.log("Unpublish local stream successfully");
		});

	```

- `client.subscribe`: Subscribe to the remote stream.

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
				remoteStream.play('agora_remote' + stream.getId());
		})
		```
	
To switch the client role:
- Set the user as the host (broadcaster)

  * `client.publish`
  * `client.subscribe`

- Switch the client role from the host to the audience

  * `client.unpublish`

- Set the user as the audience

  * `client.subscribe`

- Switch the client role from the audience to the host

  * `client.publish`

> Create and initialize the stream before calling the `client.publish` method to publish the local audio and video stream. See [Publish and Subscribe to Streams](../../cn/Interactive%20Broadcast/publish_web_video.md).

## Next Steps

Once the client role is switched to the host, you can start a live broadcast with the following step:

- [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_web_live.md)

For other functions such as manipulating the audio volume, audio effect, or video resolution, you can refer to the following sections:

- [Adjust the Volume](../../en/Interactive%20Broadcast/volume_web.md)
- [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/effect_mixing_web.md)
- [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_web.md)
