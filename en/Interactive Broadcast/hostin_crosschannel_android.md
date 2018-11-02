
---
title: Host In Across Channels
description: 
platform: Android
updatedAt: Fri Nov 02 2018 02:31:15 GMT+0000 (UTC)
---
# Host In Across Channels
# Hosting In Across Channels

In an ordinary live broadcast, hosts in the same channel can host in for interaction, while hosts from different channels cannot. This page provides solutions for users to host in from different channels:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Solution</strong></td>
<td><strong>Positive</strong></td>
<td><strong>Negative</strong></td>
</tr>
<tr><td><a href="#server_hostin">Host In at the Server</a></td>
<td>Hosts from different app channels can host in for interaction</td>
<td>Need to call additional API methods and ensure that the API call sequence is correct.</td>
</tr>
<tr><td><a href="#client_hostin">Host In at the Client</a></td>
<td>Hosts from different app channels can host in for interaction</td>
<td>Need to add signaling, and when the hosts from different channels cancel the host-in, they must return to their previous respective states.</td>
</tr>
</tbody>
</table>


<a id = "server_hostin"></a>
## Host In at the Server

For example, hosts A, B, and C join the same Agora channel, but different audience of each hosts pull different RTMP streams.

The channel of host A and audience A is one app live channel. Cross-channel host in means hosting in from different app live channels, not from different Agora channels.

The audience of host A pulls the RTMP stream of the app live channel with the layout set according to host A. The following figure shows the Channel A audience displaying host A to be bigger than hosts B and C:

<img alt="../_images/server_hostin_en.jpg" src="https://web-cdn.agora.io/docs-files/en/server_hostin_en.jpg" style="width: 590.5px; height: 787.5px;"/>


### Preparation

1.  Enable an RTMP stream according to [Pushing Streams to the CDN](../../en/Quickstart%20Guide/push_stream_android.md).

2.  Set up a web server.

3.  Create an HTML file with players built in.


### Host

1.  Hosts A, B, and C call the `configPublisher` method respectively to configure push-stream for CDN live. For example,

**Host A:**

```
    PublisherConfiguration:
      owner: true
      size: 360,640
      framerate: 15
      bitrate: 500
      defaultLayout: 1
      lifecycle: STREAM_LIFE_CYCLE_BIND2OWNER
      publisherUrl: rtmpPushUrlA
```

**Host B:**

```
  PublisherConfiguration:
      owner: true
      size: 360,640
      framerate: 15
      bitrate: 500
      defaultLayout: 1
    lifecycle: STREAM_LIFE_CYCLE_BIND2OWNER
      publisherUrl: rtmpPushUrlB
```

**Host C:**

```
PublisherConfiguration:
    owner: true
    size: 360,640
    framerate: 15
    bitrate: 500
    defaultLayout: 1
  lifecycle: STREAM_LIFE_CYCLE_BIND2OWNER
    publisherUrl: rtmpPushUrlC
```

2.  Hosts A, B, and C call the `joinChannel` method to join the same Agora channel:


### Audience

The audience does not need to call the `joinChannel` method to join the app channel created by the host. The audience only needs to access the app channel URL for CDN Live, and can share the link on different social media platforms.

When the hosts join the same Agora channel, the audience can see different layouts due to different RTMP streams being pulled:

-   The audience sees the layout set by default in `defaultLayout` defined by the hosts when calling the `configPublisher` method.

<a id = "client_hostin"></a>
## Host In at the Client

Host in at the client according to the following signaling sequence:

1.  Host A sends a host-in request to host B.

2.  Host B accepts the host-in request.

3.  Host B informs all audience in Channel B of the request.

4.  All audience in Channel B exit Channel B, then join Channel A.

5.  Host B exits Channel B, and joins Channel A.


Now, Host A and B can host in for interaction. The process of multiple users hosting in will be the same. Once host in ends, the host and audience in Channel B will return to their original states.


