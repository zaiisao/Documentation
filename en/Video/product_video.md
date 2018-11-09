
---
title: Agora Video Overview
description: 
platform: All Platforms
updatedAt: Fri Nov 09 2018 11:02:53 GMT+0000 (UTC)
---
# Agora Video Overview
The Agora Native SDK for Video Call enables easy and convenient one-to-one or one-to-many video calls that feature high stability and low latency. It is applicable to scenarios in the entertainment and education industries.

The major difference between Agora Video Call and [Agora Video Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms) is: 
* During an Agora Video Call that highlights fluency and low latency, all users are of the same role and can talk to each other freely. A typical scenario of the Agora Voice Call is video conference call for multi persons. 
* During an Agora Video Interactive Broadcast that highlights high voice quality, users are divided into Host and Audience. Only the host can talk. If a user wants to talk, he must change his role as host first. A typical scenario of the Agora Video Interactive Broadcast is the online interactive class.

## Functions and Scenarios

The Agora Native SDK for Video Call boasts a flexible combination of functions for different scenarios.

| Function                              | Description                                                  | Scenario                                                     |
| ----------------- | ------------------------------------------------------------ | --------------------------------------- |
| Audio Mixing          | Send local and online audio and user vioce to other audience in the channel. | <li>Online KTV <li>Interactive music classes for children |
| Screen Sharing             | Enable hosts to share their screen to audience in the channel.                         | Interactive online classes                                                  |
| Modify Raw Data   | Enable developers to obtain and modify the voice or video raw data and to create special effects, such as voice change. | <li>Change the voice in an online chatroom <li>Facial beautification in a video call                  |
| Customize Video Source and Renderer | Enable access to customized video sources and renderers, which means, besides the system camera, users can utilize their self-built camera, videos from screen sharing or files, etc. In this way, they are able to process videos more flexibly, such as to adding beauty effects and filters. | <li>Users want to use a customized beauty effect library or pre-processing library;<li>The App already has built-in image and video modules;<li>Developers want to use other video sources, such as a recorded video, besides the system camera;<li>Some video capture devices are exclusive, so they need flexible device management strategies to avoid conflicts with other services. |

## Key Properties

| Property                                          | Agora Live Broadcast Specifications                          |
| ------------ | ------------------------------------------------------------ |
| SDK Package Size                                  | 3.69 M - 7.75 M                                              |
| Capacity     | 7 peopel (To support more people, use [Agora Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).) |
| Video Profile                                     | <li>SDK video source: up to 1080p, 60fps<li>Custom vide source: up to 4K |
| Audio Profile                                     | <li>Sample rate: 16 KHz - 48 KHz<li>Support for mono and stereo audio |
| Audio Anti Packet Loss Rate                       | 70% (both uplink and downlink)                               |

## Compatibility

The Agora Native SDK for Video Call is supported on a variety of platforms, including iOS, Android, Windows, macOS, Linux, Web and Wechat Miniapp, and allows cross-platform connection. Below is the list of supported versions for different platforms.

<table>
  <tr>
    <th>Platform</th>
    <th>Supported Version</th>
  </tr>
  <tr>
    <td>Android</td>
    <td>4.1+</td>
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
    <td>WeChat Mini App</td>
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
