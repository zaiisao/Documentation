
---
title: Switch the Client Role
description: 
platform: Android
updatedAt: Thu Dec 13 2018 21:53:21 GMT+0800 (CST)
---
# Switch the Client Role
Before switching the client role, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/android_video.md).

## Implementation

A live broadcast channel consists of two user roles: 

-   Host (Broadcaster): A host receives and publishes the voice streams, and interacts with other hosts using voice.
-   Audience: An audience can only hear the hosts.

You can call the `setClientRole` method to set the user role. 

```
mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
```

> You can call the `setClientRole` method before joining a live broadcast channel or during a live broadcast:
> 
>  - Before joining the channel: Set the client role as the host or audience.
>  -  During a live broadcast: Switch the user role from an audience to the host or vice versa.

If two users join a live broadcast channel as hosts:

1. User A calls the `setClientRole` method to set the user role as the host, and calls the `joinChannel` method to join the channel.

   ```Java
   //Set the user role as the host.
   mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
   
   //Create and join a channel.
   mRtcEngine.joinChannel(null, "channelName", null, myUid)
   ```
	 
2. User B calls the `setClientRole` method to set the user role as the host, and calls the `joinChannel` method to join a channel.

   ```Java
   //Set the user role as the host.
   mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
   
   //Create and join a channel.
   mRtcEngine.joinChannel(null, "channelName", null, myUid)
   ```
	 
User A joins the channel as a host and user B joins as an audience. If user B wants to switch to a host:

1. User A calls the `setClientRole` method to set the user role as the host, and calls the `joinChannel` method to join a channel.

   ```Java
   //Set the user role as the host.
   mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER)
   
   //Create and join a channel.
   mRtcEngine.joinChannel(null, "channelName", null, myUid)
   ```
	 
2. User B calls the `joinChannel` method to join the channel as an audience, and calls the `setClientRole` method to switch the user role to the host.

   ```Java
   //Create and join a channel.
   mRtcEngine.joinChannel(null, "channelName", null, myUid)
   
   //Set the user role as the host.
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

