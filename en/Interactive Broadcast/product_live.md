
---
title: Agora Interactive Broadcast Overview
description: 
platform: All Platforms
updatedAt: Wed Nov 14 2018 19:45:57 GMT+0000 (UTC)
---
# Agora Interactive Broadcast Overview
The Agora Native SDK for Interactive Broadcast enables one-to-many and many-to-many audio or video live streaming. Distinguished from the traditional CDN live broadcast which only allows one-way communication from hosts to audience, our SDK empowers audience to interact with hosts through ["hosting-in"](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#hosting-in), just like a viewer jumps on to the stage in the middle of a play and starts to perform. The Agora Native SDK for Interactive Broadcast is applicable to scenarios that encourage active engagement, such as game-playing, online classes for students in small groups, and the Q&A session during the E-commerce live streaming. You can also use this SDK for one-to-one video calls that require high image quality.

## Functions and Scenarios

The Agora Native SDK for Interactive Broadcast Agora boasts a flexible combination of functions for different scenarios.

| Function                              | Description                                                  | Scenario                                                     |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Hosting-in at the Client Side         | Enable audience to change their role as co-host and interact with the existing host. | <li>During a large-scale live stream, the hosts can invite the audience to interact with them. <li>Play online games such as Murder Mystery Game and Werewolf Killing. |
| Hosting-in across Channels            | Enable hosts to interact with each other across channels.    | PK Hosting                                                   |
| Audio Mixing                          | Send local and online audio and user vioce to other audience in the channel. | <li>Online KTV <li>Interactive music classes for children    |
| Screen Sharing                        | Enable hosts to share their screen to audience in the channel. | <li>Interactive online classes<li>Live streaming of gaming hosts |
| Modifying Raw Data                    | Enable developers to obtain and modify the voice or video raw data of the SDK engine to create special effects, such as voice change. | <li>Change the voice in an online voice chatroom<li>Facial beautification in a live stream |
| Injecting Online Media Stream         | Inject an external audio or video stream to an ongoing live broadcast channel, so the host and audience in the channel can listen to or watch the stream while interacting with each other. The attributes of video source can be set. | <li>The host and audience watch a movie or game together.    |
| Customizing Video Source and Renderer | Enable access to customized video sources and renderers, which means, besides the system camera, users can utilize their self-built camera, videos from screen sharing or files, etc. In this way, they are able to process videos more flexibly, such as to adding beauty effects and filters. | <li>Users want to use a customized beauty effect library or pre-processing library;<li>The App already has built-in image and video modules;<li>Developers want to use other video sources, such as a recorded video, besides the system camera;<li>Some video capture devices are exclusive, so they need flexible device management strategies to avoid conflicts with other services. |
| Pushing Streams to CDN                | Send audio and video contents of your channel to other RTMP servers via CDN;<li>Start or stop publishing at any time;<li>Add or remove an address while continuously publishing the stream; <li>Adjust the picture-in-picture layout. | <li>Send the live stream to Wechat or Weibo;<li>Enable more people to watch the live stream when the number of audience in the channel exceeds the limit. |

For more applications, see the following sample codes:

- [PK Hosting](https://github.com/AgoraIO/ARD-Agora-Online-PK/blob/master/README.zh.md)
- [Trivia Game](https://github.com/AgoraIO/HQ)
- [Online KTV](https://github.com/AgoraIO/Agora-Online-KTV/blob/master/README.zh.md)
- [Online Voice Chatroom](https://github.com/AgoraIO-Usecase/Chatroom)
- [Clip Doll Machine](https://github.com/AgoraIO/Wawaji)
- [Murder Mystery Game](https://github.com/AgoraIO-Usecase/Murder-Mystery-Game)

## Key Properties

| Property                                          | Agora Live Broadcast Specifications                          |
| ------------------------------------------------- | ------------------------------------------------------------ |
| SDK Package Size                                  | 3.69 M - 7.75 M                                              |
| Host Capacity                                     | 17 people                                                    |
| Audience Capacity                                 | One million people                                           |
| Video Profile                                     | <li>SDK video source: up to 1080p, 60fps<li>Custom vide source: up to 4K |
| Audio Profile                                     | <li>Sample rate: 16 KHz - 48 KHz<li>Support for mono and stereo audio |
| Audio Anti Packet Loss Rate                       | 70% (both uplink and downlink)                               |
| Latency between Host and Hosting-in Audience/Host | 200 -600 ms                                                  |

## Compatibility

The Agora Native SDK for Interactive Broadcast is supported on a variety of platforms, including iOS, Android, Windows, macOS, Linux, Web and Wechat Miniapp, and allows cross-platform connection. Below is the list of supported versions for different platforms.

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
