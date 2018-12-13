
---
title: Switch the Client Role
description: 
platform: Android
updatedAt: Thu Dec 13 2018 21:53:21 GMT+0000 (UTC)
---
# Switch the Client Role
Before switching the client role, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/android_video.md).

## Implementation

The channel includes two user roles once you set the channel profile as live broadcast. Call `setClientRole` method to set the user roles according to the needs. The differences of the two roles are:

-   Host (Broadcaster): A host can receive and publish the voice streams. The host can also interact with other hosts using voice.
-   Audience: An audience can only hear the host(s).

```
mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
```

> This method can be called either before joining a live broadcast channel or during a live broadcast:
> 
>  - Before joining the channel: Set the client role as the broadcaster or audience.
>  -  During a live broadcast: Switch the user role from audience to broadcaster or vice versa.

Suppose two users join a live broadcast channel. If both users join the channel as hosts:

1. User A calls `setClientRole`, sets the user role as boradcaster, and calls `joinChannel` to join the channel.

   ```Java
   //Set the user role as broadcaster
   mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
   
   //Create and join a channel
   mRtcEngine.joinChannel(null, "channelName", null, myUid)
   ```
	 
2. User B calls `setClientRole`, sets the user role as broadcaster, and calls `joinChannel` to join the channel.

   ```Java
   //Set the user role as broadcaster
   mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
   
   //Create and join a channel
   mRtcEngine.joinChannel(null, "channelName", null, myUid)
   ```
	 
If one user joins the channel as a host, and the other as an audience. A while later, the other user wants to swtich to a host:

1. User A calls `setClientRole`, sets the user role as host, and then calls `joinChannel` to join a channel.

   ```Java
   //Set the user role as boradcaster
   mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
   
   //Create and join a channel
   mRtcEngine.joinChannel(null, "channelName", null, myUid)
   ```
	 
2. User B calls `joinChannel`, joins the channel as an audience, and then calls `setClientRole` to switch the user role to host.

   ```Java
   //Create and join a channel
   mRtcEngine.joinChannel(null, "channelName", null, myUid)
   
   //Set the user role as broadcaster
   mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
   ```

## Next Steps
Once the client role is switched to the host, you can start a live broadcast with the following step:

- [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_android_live.md)

For other functions such as manipulating the audio volume, audio effect, or video resolution, you can refer to the following sections:

- [Adjust the Volume](../../en/Interactive%20Broadcast/volume_android.md)
- [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/effect_mixing_android.md)
- [Use In-Ear Monitoring](../../en/Interactive%20Broadcast/in-ear_android.md)
- [Adjust the Pitch and Tone](../../en/Interactive%20Broadcast/voice_effect_android.md)
- [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_android.md)

