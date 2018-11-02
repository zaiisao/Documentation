
---
title: Switch the Client Role
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 16:31:03 GMT+0000 (UTC)
---
# Switch the Client Role
An interactive broadcast channel consists of two user roles: the host and the audience. Call the <code>setClientRole</code> method to set the user role as broadcaster or audience according to your needs. The differences of the two roles are:

-   Host (Broadcaster): A host receives and publishes the audio streams. The host can also interact with other hosts using voice.
-   Audience: An audience receives the audio streams of the host(s).

```
int nRet = m_lpAgoraEngine->setClientRole(role);
```

> This method can be called either before joining a live broadcast channel or during a live broadcast:
> 
>  - Before joining the channel: Set the client role as the broadcaster or audience.
>  -  During a live broadcast: Switch the user role from audience to broadcaster or vice versa.

Suppose two users join a live broadcast channel. If both users join the channel as hosts:

1. User A calls `setClientRole`, sets the user role as boradcaster, and calls `joinChannel` to join the channel.

   ```cpp
   //Set the user role as broadcaster
   int nRet = m_lpAgoraEngine->setClientRole(role);
   
   //Create and join a channel
   LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
   nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
   ```
	 
2. User B calls `setClientRole`, sets the user role as boradcaster, and calls `joinChannel` to join the channel.

   ```cpp
   //Set the user role as broadcaster
   int nRet = m_lpAgoraEngine->setClientRole(role);
   
   //Create and join a channel
   LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
   nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
   ```

If one user joins the channel as a host, and the other as an audience. A while later, the other user wants to swtich to a host:

1. User A calls `setClientRole`, sets the user role as host, and then calls `joinChannel` to join a channel.

   ```cpp
   //Set the user role as broadcaster
   int nRet = m_lpAgoraEngine->setClientRole(role);
   
   //Create and join a channel
   LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
   nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
   ```

2. User B calls `joinChannel`, joins the channel as an audience, and then calls `setClientRole` to switch the user role to host.

   ```cpp
//Create and join a channel
   LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
   nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
	 
   //Set the user role as broadcaster
   int nRet = m_lpAgoraEngine->setClientRole(role);
   ```
