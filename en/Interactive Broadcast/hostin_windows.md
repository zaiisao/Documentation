
---
title: Host In
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 04:19:27 GMT+0000 (UTC)
---
# Host In
This page describes how to implement the host-in function at the client side.

In the current market, a live broadcast pushes the host’s voice and video data to the CDN cloud through the RTMP protocol. This results in high latency with the host and audience unable to interact with each other. With Agora’s host-in function and the push-stream function provided by live broadcast vendors or Agora servers, you can implement the following functions:

-   The audience can interact with the hosts

-   Multiple users can interact with each other

-   Users who have customization requirements can capture/modify the raw video data


The host-in function is implemented at the client. If you need the push-stream function, see [Pushing Streams to the CDN](../../en/Quickstart%20Guide/push_stream_windows.md) to decide on whether to push streams at the client or server.

This page describes two host-in scenarios with two users.

## Preparation

-   For the voice host-in function, implement live voice broadcast according to [Starting a Live Voice Broadcast](../../en/Quickstart%20Guide/broadcast_audio_windows.md).

-   For the video host-in function, implement live video broadcast according to [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_windows.md).


### Example 1

Both users join the channel as hosts:

1.  User A calls `setClientRole` to set the user role as host, and then calls `joinChannel` to the join channel.

2.  User B calls `setClientRole` to set the user role as host, and then calls `joinChannel` to the join channel.


Users A and B can start to host in.

### Example 2

One user joins the channel as a host, and the other as an audience before switching to a host:

1.  User A calls `setClientRole` to set the user role as host, and then calls `joinChannel` to join a channel.

2.  User B calls `joinChannel` to join the channel as an audience, and then calls `setClientRole` to switch the user role to host.


Users A and B can start to host in.


