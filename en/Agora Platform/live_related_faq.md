
---
title: Live-related Issues
description: 
platform: Live-related Issues
updatedAt: Thu Nov 01 2018 09:22:09 GMT+0000 (UTC)
---
# Live-related Issues
# Live-related Issues

### What is the difference between the CDN (Content Delivery Network) and the Agora Interactive Broadcast?

The CDN is traditional broadcast with high latency and no interaction.

The Agora Interactive Broadcast enables real-time interactions between hosts and the audience with low latency.

### Does Agora support overseas broadcast?

Yes, Agora supports overseas broadcasts With data centers deployed globally.

### What makes the architecture of the Agora Live Broadcast unique?

Agora uses UDP (User Datagram Protocol) instead of the traditional and high-latency TCP (Transmission Control Protocol). UDP guarantees low latency and real-time interaction.

### How does Agora support high concurrent usage?

Agora has deployed over 100 data centers globally with thousands of servers to support billions of concurrent users. Meanwhile, Agora has partnerships with top CDN vendors to support high concurrent usage for CDN Live.

### How to ensure the channel name used is unique?

The app decides. If you specify the same channel name as others, then you will join the same Agora channel.

### How can I kick people out of a specific live broadcast channel?

You are allowed to kick people out of a channel through the Agora Signaling System. This function for the server side is still under development.

### Why can’t hosts see each other after joining the same Agora channel?

If this happens, do the following:
1. Ensure both hosts use the same App ID.
2. Ensure both hosts joined the same channel with the same channel name.
3. Ensure the network connections for both hosts are good.

### Does Agora support voice-only live broadcasts?

Yes.

### What is the recommended resolution to be used?

Currently 1080p is supported as the maximum resolution. However, the recommended resolution varies according to the different scenarios and the processing capacity of the receiver.

The recommended resolution for a mobile phone is 360 x 640, if the bitrate or resolution that the audience client receives is too high, power consumption will be increased, causing CPU processing bottleneck issues, resulting in issues such as choppy video playback.

Once the hardware capabilities are enhanced at the audience client side, Agora will customize dual resolutions. For example, 720p for high-end mobile phones and tablets, and 360p for low-end mobile phone.

### What are the advantages of Agora host in for live broadcasts?

* Agora supports multiple users hosting in, and currently supports up to seven video hosts in a single Agora channel.
* Agora host in is based on UDP technology with low latency.
* Users can host in and out anytime during a live broadcast.

### What is the difference between Client Host In and Server Host In?

Host-in is implemented on the client side.

The only difference is the way to blend the pictures:

<table>
  <tr>
    <th>Method</th>
    <th>Disadvantage</th>
    <th>Advantage</th>
  </tr>
  <tr>
    <td>Blending pictures on the client side</td>
    <td>Higher demand on the bandwidth and device</td>
    <td>More accurate plan on the layouts and host-in strategies</td>
  </tr>
  <tr>
    <td>Blending pictures on the Agora server side</td>
    <td>Insufficient support on layout and host-in strategies</td>
    <td>Lower latency, and lower demand on the device and bandwidth</td>
  </tr>
</table>

### What does Single- and Dual-stream audience/host mean?

In a live broadcast, Agora supports two user roles: Host and audience, which support single- and dual-stream modes (high and low streams defined by the resolution, frame rate, and bitrate).

* Single- and Dual-Stream Host: A single-stream host sends one stream, while a dual-stream host sends two streams.
* Single- and Dual-Stream Audience: Both single- and dual-stream audience receive one stream from each host. A single-stream audience receives a high stream by default, while a dual-stream audience receives a low stream by default. The stream type can be changed by calling setRemoteVideoStreamType(Android/Windows) or setRemoteVideoStream(iOS/Mac).

Do not mix the usage of single- and dual-stream roles:

<table>
  <tr>
    <th>Role and Mode</th>
    <th>Single-Stream Host</th>
    <th>Dual-Stream Host</th>
  </tr>
  <tr>
    <td>Single-Stream Audience</td>
    <td>Supported</td>
    <td>Not Supported</td>
  </tr>
  <tr>
    <td>Dual-Stream Audience</td>
    <td>Not Supported</td>
    <td>Supported</td>
  </tr>
</table>

### Does Dual-stream audience mean that the audience can receive both streams from the host?

No, it only means that the audience can select receiving one stream from the two streams sent out by each host:
 
* Each audience can only receive one stream from each host.
* The stream (high or low) received from the hosts is determined by the settings when calling setRemoteVideoStreamType.
* A single-stream audience receives a high stream by default, while the dual-stream audience receives a low stream by default.

### Can users mix different host and audience modes?

No, only the following combinations are supported:

* Single-stream host + single-stream audience
* Dual-stream host + dual-stream audience

### Does Agora provide any host-in authentication? If not, what happens if the app is hacked?

Agora provides host-in authentication to ensure the live broadcast will not be affected if the app is hacked. This function will be available in later releases, but some customers are using this function now with customized requests. For details on enabling this function now, contact sales-us@agora.io.

### What happens when push stream fails?

Push stream retries automatically every two seconds once it fails.

### Why is push stream/recording not smooth for the last 30 seconds?

To prevent the RTMP stream from being disconnected due to the host being disconnected or temporarily offline, the recording service reserves 30 seconds to buffer the static data, which means completely exiting the push-stream recording 30 seconds after the host is disconnected.

### What is the difference between Agora Cloud and CDN RTMP?

Most CDN + RTMP technologies for live broadcast allow users to watch the live broadcast in a web browser once a flash player is installed, which lowers the audience’s threshold. Agora provides a solution for the Agora Cloud, host, and audience to have the same real-time communication quality as an individual line with:

* Private voice and video coding
* Private transport protocol
* Private node deployment
* Private transmission algorithms

See the following table for details:

<table>
  <tr>
    <th>Name</th>
    <th>Normal CDN + RTMP</th>
    <th>Agora Cloud</th>
  </tr>
  <tr>
    <td>Video Encoding and Decoding</td>
    <td>H.264</td>
    <td>Private</td>
  </tr>
  <tr>
    <td>Audio Encoding and Decoding</td>
    <td>AAC</td>
    <td>Private</td>
  </tr>
  <tr>
    <td>Transport Protocol</td>
    <td>TCP based on RTMP</td>
    <td>Private protocol based on UDP</td>
  </tr>
  <tr>
    <td>Transmission Algorithm</td>
    <td>TCP</td>
    <td>Private algorithm for fixing packet loss and adjusting the bitrate automatically according to the current bandwidth</td>
  </tr>
  <tr>
    <td>Picture-in-picture</td>
    <td>Fixed</td>
    <td>Can be adjusted dynamically</td>
  </tr>
</table>

To meet developer requirements, Agora enables the function of CDN Live (used for social sharing) with CDN vendors.

### Why aren’t the displayed videos tiled, but layered on the Windows platform (part of the videos cannot display)?

If the displayed videos are not tiled but layered, and do not have the same parent window, then they may overlap each other. In this case, you need to set the following style for the parent windows by calling the Win32 API ::SetWindowLong:

```
LONG styleValue = ::GetWindowLong(parentWindow.GetSafeWnd(), GWL_STYLE);
::SetWindowLong(parentWindow.GetSafeHwnd(), styleValue | WS_CLIPSIBLINGS | WS_CLIPCHILDREN);
```

### Why is the actual layout resolution 640 x 352 when I set it to 640 x 360?

Some Android devices are limited to supporting video widths divided by 16 by some encoders. Hence, Agora has defined the resolutions to multiples of 16. This is why when you set the width to 360, the actual width is 352.

### Why is the layout one-pixel displaced compared to the actual layout?

When the lowest video decoder is in YUV420 format, the sampling rate of YUV is 2:1:1 in the two directions of the width and height. Hence, the width, height, and moving distance must be even. However, since our customized layout uses relative values, the width, height, and position of each anchor are calculated to be even when calculating the transform, resulting in a deviation of one pixel from the data expected by the user.

### Why is there a cross-domain issue when using a CDN test address?

The domain used by the test address applies to the test environment. Since CDN is not configured with cross.xml, cross-domain issues occur.

### What H5/Flash player is recommended for CDN Live?

It is recommended to use video.js or jwplayer. You can refer to the following page for testing: http://broadcastapp.agora.io/test.html.

### How do I enable or disable CDN Live?

For now, contact sales-us@agora.io to enable or disable CDN Live. This function will be enabled in the Dashboard later.

### How do I get the channel list for the current CDN Live?

Agora does not provide the CDN Live channel list. The application layer may be able to provide it.

### Can we specify which channel to enable for CDN Live?

No, currently this function is not supported. Once the function of CDN Live is enabled, all channels will support CDN Live.

### How do I get the current state of the bypass channel?

Use the RESTful API to get the status of the bypass channel.

### Can I specify the resolution and bitrate for CDN Live?

Yes, but pushing the stream with the designated resolution and bitrate requires encoding and decoding again which may require extra resources:

* Scenario 1: Only one host in the live broadcast; requires extra resources.
* Scenario 2: More than one host in the live broadcast; blending pictures requires encoding and decoding as well and requires no extra resource.

### What is the difference between the CDN Live and Agora SDK audience?

Agora Live Broadcast includes three users:

* Host
* Advanced Audience
* Normal Audience

The host and advanced audience needs to use the Agora SDK, which means only the users who use the application integrated with the Agora SDK can be called the host or advanced audience. A normal audience only needs a web browser.

Compared to a normal audience, an advanced audience has the following advantages:

* Lower latency when watching the live broadcast.
* Low packet loss.
* Switch the user role from the audience to host for real-time interaction.

### Can we push the stream to our own CDN?

Contact sales-us@agora.io for details.

### Does voice live broadcast support CDN Live?

Yes.

### Is there a traffic limit on CDN Live?

There are two types of URLs for CDN Live:

* Test: The CDN Live function is enabled immediately after registration, but it may be unstable and only valid for one week. It is free, and supports up to five streams simultaneously.
* Official: The CDN Live function is enabled four business days after registration. It is stable, and valid for the duration agreed in the contract with no restrictions on concurrency. It is not free.

### Why is the latency of HLS higher than RTMP?

HLS includes an m3u8 index file and a TS media slicing file. The m3u8 index file is at least three slice files while the shortest TS file is two seconds a slice; which means the latency is at least 6 seconds. RTMP is a direct pass with a short buffer, so the latency is less than HLS.

### Why is there no image, but only audio in CDN Live and the CDN Live recordings?

Check whether the API setVideoCompositingLayout is called correctly.

### How long is the Channel Key valid for? Does it impact the live broadcast once it expires?

The Channel Key includes two fields related to time/duration:

* Authorization Timestamp: The timestamp, represented by the number of seconds elapsed since 1/1/1970. The user can use the Channel Key to join a channel within five minutes after the Channel Key is generated. If the user does not join a channel after five minutes, this Channel Key is no longer valid.

* Expiration Timestamp: Indicates the exact time when a user can no longer use the Agora service (for example, when someone is forced to leave the live broadcast). It is infinite by default but users can customize it according to actual needs.

### Can I capture the video and ask the Agora SDK to process it afterwards?

Contact sales-us@agora.io for details.

### Does the Agora SDK support ReplayKit?

ReplayKit is the built-in screen recording function of iOS 10, but the Agora SDK currently does not support it.

However, users can capture the video and ask the Agora SDK to process it.

### How do I switch the main host window when a PC has multiple cameras connected?

For a PC with multiple external USB cameras, the users can capture the source video from different cameras, then input them to the SDK for screen switching, or directly hot swap the screen on the application side. The Agora SDK supports hot swap among multiple USB cameras on a PC.

### Can I support one host with several simultaneous video streams?

Yes, to implement it in the following methods:

* The app captures the screen and host video, mixes them as one stream, and inputs it in the Agora SDK (it is not recommended to blend the host video stream and the SDK built-in screen-capture stream, due to efficiency loss. Agora will provide technical support for how to capture the screen using a PC client.)

* The user joins as a host with a PC app, and joins as an audience with a mobile app, then switches from the audience in the mobile app to the host to be displayed as a small window in the live broadcast.

* In a PC app, switch from an audience to a host to display the host video in the live broadcast.

### Does Agora support the watermark function in live broadcast video?

No, but you can implement this function using the preprocessing and postprocessing interfaces.

### Does Agora support detecting sexually explicit content?

Yes, Agora supports the following two functions:

* Identify the sensitive content.
* Capture the screenshots of the sensitive content and send them to the developer.

### Can I get the duration of the audience activities in the live channel?

You can calculate it through the signaling layer. Agora will provide a non real-time interface later.


