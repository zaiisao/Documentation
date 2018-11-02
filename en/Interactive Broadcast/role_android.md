
---
title: Switch Client Role
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:16:01 GMT+0000 (UTC)
---
# Switch Client Role
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
