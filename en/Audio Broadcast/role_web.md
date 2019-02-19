
---
title: Switch the Client Role
description: 
platform: Web
updatedAt: Tue Feb 19 2019 10:50:25 GMT+0000 (UTC)
---
# Switch the Client Role

A live broadcast channel consists of two user roles:
- Host (Broadcaster): A host receives and publishes the voice streams, and interacts with other hosts using voice.
- Audience: An audience can only hear the hosts.

Web v2.5.1 provides you with the `setClientRole` method to switch the client role.
> Please ensure that you call the `setClientRole` method in the live-broadcast mode ([mode](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.clientconfig.html#mode) is set to `live`). In the communication mode ([mode](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.clientconfig.html#mode) is set to `rtc`), all client user is `host` by default and this method cannot be called.

## Implementation

Before switching the client role, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md).


```
// Sets the client role to “host” or “audience”.
client.setClientRole("host", function() {
  console.log("setHost success");
}, function(e) {
  console.log("setHost failed", e);
})

...

// The client-role-changed callback.
client.on("client-role-changed", function(evt) {
  console.log("client-role-changed", evt.role);
});

```

You can call `setClientRole` before or after you join a channel:
- Call this method before joining a channel to set the client role to `host` or `audience`.
- Call this method after joining a channel to switch the client role between `host` and `audience`.

When the client role is changed, you receive the `client-role-changed` callback.
	

### Considerations

- If an audience calls [`client.publish`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#publish) ，`role` is automatically set to `host`;
- If a host calls [`client.unpublish`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#unpublish) ，`role` is automatically set to `audience`.


## Next Steps

Once the client role is switched to the host, you can start a live broadcast with the following step:

- [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_web_live.md)

For other functions such as manipulating the audio volume, audio effect, or video resolution, you can refer to the following sections:

- [Adjust the Volume](../../en/Interactive%20Broadcast/volume_web.md)
- [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/effect_mixing_web.md)
- [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_web.md)
