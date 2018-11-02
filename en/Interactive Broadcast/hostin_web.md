
---
title: Host In
description: 
platform: Web
updatedAt: Fri Nov 02 2018 02:37:39 GMT+0000 (UTC)
---
# Host In
# Hosting In

This page describes how to implement the host-in function at the client side.

In the current market, a live broadcast pushes the host’s voice and video data to the CDN cloud through the RTMP protocol. This results in high latency with the host and audience unable to interact with each other. With Agora’s host-in function and the push-stream function provided by live broadcast vendors or Agora servers, you can implement the following functions:

- The audience can interact with the hosts
- Multiple users can interact with each other

The host-in function is implemented at the client. If you need the push-stream function, see [Pushing Streams to the CDN](../../en/Quickstart%20Guide/push_stream_web.md) to decide on whether to push streams at the client or server.

## Preparation

- For the voice host-in function, implement voice live broadcast according to [Starting a Live Voice Broadcast](../../en/Quickstart%20Guide/broadcast_audio_web.md).
- For the video host-in function, implement video live broadcast according to [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_web.md).

### Example

1. User A calls `client.join` to join the channel, and then calls `client.publish` to set the user role as host.
2. User B calls `client.join` to join the channel as audience \(by default\), and then calls `client.publish`  to start hosting in. To stop hosting in, call `client.unpublish`.
