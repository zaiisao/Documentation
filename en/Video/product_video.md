
---
title: Agora Video Overview
description: 
platform: All Platforms
updatedAt: Mon Feb 11 2019 08:32:50 GMT+0000 (UTC)
---
# Agora Video Overview
The Agora Native SDK for Video Call enables easy and convenient one-to-one or one-to-many calls and supports voice-only and video modes.

The difference between an Agora Video Call and [Agora Video Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms) is: 
* An Agora Video Call prioritizes fluency and low latency. All users are the same role and can talk to each other freely. A typical scenario of an Agora Video Call is a video conference call for many persons. 
* An Agora Video Interactive Broadcast prioritizes high voice quality. Users can be the host or audience, where only the host can talk. A user who wants to talk must change the role to a host. A typical scenario of an Agora Video Interactive Broadcast is an interactive online class.

## Functions and Scenarios

The Agora Native SDK for Video Call boasts a flexible combination of functions for different scenarios.

| Function                              | Description                                                  | Scenario                                                     |
| ----------------- | ------------------------------------------------------------ | --------------------------------------- |
| Audio Mixing          | Sends the local and online audio with the user's voice to other audience members in the channel. | <li>Online KTV. <li>Interactive music classes. |
| Screen Sharing             | Enables hosts to share their screen to the audience in the channel.                         | Interactive online classes.                                                  |
| Modify the Raw Data   | Enables developers to obtain and modify the raw voice or video data and to create special effects, such as a voice change. | <li>To change the voice in an online chatroom. <li>Image enhancement in a video call.                  |
| Customize the Video Source and Renderer | Enables customization of the video sources and renderers. This allows users to use self-built cameras and videos from screen sharing or files to process videos, such as for image enhancement and filtering. | <li>To use a customized image enhancement library or pre-processing library.<li>To customize the application's built-in image and video modules.<li>To use other video sources, such as a recorded video.<li>To provide flexible device management for exclusive video capture devices to avoid conflicts with other services. |

## Key Properties

| Property                                          | Agora Video Call Specifications                          |
| ------------ | ------------------------------------------------------------ |
| SDK Package Size                                  | 3.69 MB to 7.75 MB                                              |
| Capacity     | Seven users (To support more users, use [Agora Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms)) |
| Video Profile                                     | <li>SDK video source: Up to 1080p @ 60 fps<li>Custom video source: Up to 4K |
| Audio Profile                                     | <li>Sample rate: 16 KHz to 48 KHz<li>Support for mono and stereo sound |
| Audio Anti-packet-loss Rate                       | 70% (uplink and downlink)                               |

## Compatibility

The Agora Native SDK for Video Call is supported on platforms such as iOS, Android, Windows, macOS, Linux, Web, and WeChat Mini-programs, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

<table>
  <tr>
    <th>Platform</th>
    <th>Supported Version</th>
  </tr>
  <tr>
    <td>Android</td>
		<td><p>4.1+</p>
		<p>The Android SDK supports the following architecture:</p>
			<ul><li>ARMv7</li>
				<li>ARM64</li>
				<li>X86</li>
				</td>
  </tr>
  <tr>
    <td>iOS</td>
    <td>8.0+</td>
  </tr>
	  <tr>
    <td>Windows</td>
    <td>XP SP3+</td>
  </tr>
  <tr>
    <td>macOS</td>
    <td>10.0+</td>
  </tr>
  <tr>
    <td>WeChat Mini-programs</td>
    <td>Support</td>
  </tr>
  <tr>
    <td>Web</td>
		<td><ul><li>Chrome 58+</li>
			<li>Firefox 56+</li>
			<li>Safari 11+</li>
			<li>Opera 45+</li>
			<li>QQ 10+</li>
            <li>360 Security Browser 9.1+</li></ul></td>
  </tr>
</table>
