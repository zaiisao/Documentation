
---
title: Agora Audio Broadcasting Overview
description: 
platform: All Platforms
updatedAt: Tue Jun 30 2020 02:09:32 GMT+0800 (CST)
---
# Agora Audio Broadcasting Overview
The Agora Native SDK for Audio Broadcasting enables one-to-many and many-to-many audio live streaming. 

The difference between an [Agora Voice Call](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and Agora Voice Interactive Broadcast is:

- An Agora Voice Call prioritizes smoothness and low latency. All users are the same role and can talk to each other freely. A typical scenario of an Agora Voice Call is a voice conference call for many persons.
- An Agora Voice Interactive Broadcast prioritizes high voice quality. Users can be the host or audience, where only the host can talk. A user who wants to talk must change the role to a host. A typical scenario of the Agora Voice Interactive Broadcast is an online trivia game.

Different from the traditional CDN live broadcast, which only allows one-way communication from the hosts to the audience, the Agora SDK for Interactive Broadcast empowers the audience to interact with the hosts through [hosting-in](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#hosting-in), like a viewer jumping onto the stage in the middle of a play to perform.

The Agora Native SDK for Interactive Broadcast is applicable to scenarios that encourage active engagement, such as game-playing, online classes for students in small groups, and Q&A sessions during E-commerce live streaming.

## Functions and Scenarios

The Agora Native SDK for Audio Broadcasting boasts a flexible combination of functions for different scenarios.

<style> table th:first-of-type {     width: 150px; } th:third-of-type {     width: 170px; }</style>

| Function                              | Description                                                  | Scenario                                                     |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Host-in at the client Side         | An audience switches to a co-host and interacts with the existing host. | <li>Large-scale live streams where hosts can invite the audience to interact with them. <li>Online games such as Murder Mystery and Werewolf Killing. |
| Host-in across channels            | Hosts interact with each other across channels.    | PK Hosting.                                                  |
| Audio mixing                          | Sends the local and online audio with the user's voice to other audience members in the channel. | <li>Online KTV. <li>Interactive music classes for children.    |
| Voice changer and reverberation     | Provides multiple presets to easily change the voice and set reverberation effects, also supports adjusting the pitch and using the equalization and reverberation modes flexibly.                    | <li>Online KTV.<li>To change the voice in an online chatroom.        |
|Spatial sound effects |	Sets the spatial sound effects for remote users to provide immersive experiences.|	FPS games. |
| Modify the raw data                    | Developers obtain and modify the raw voice data of the SDK engine to create special effects, such as a voice change. | <li>To change the voice in an online voice chatroom.<li>Image enhancement in a live stream. |
| Inject an online media stream         | Injects an external audio stream to an ongoing live broadcast channel. The host and audience in the channel can listen to or watch the stream while interacting with each other. You can set the attributes of the audio source. | <li>The host and audience listening to a concert together.    |
| Push streams to the CDN                | Sends the audio of your channel to other RTMP servers through the CDN:<li>Starts or stops publishing at any time.<li>Adds or removes an address while continuously publishing the stream. <li>Adjusts the picture-in-picture layout. | <li>To send a live stream to WeChat or Weibo.<li>To allow more people to watch the live stream when the number of audience members in the channel reached the limit. |

See the following sample code for application scenarios:

- [PK Hosting](https://github.com/AgoraIO/ARD-Agora-Online-PK/blob/master/README.zh.md)
- [Online KTV](https://github.com/AgoraIO/Agora-Online-KTV/blob/master/README.zh.md)
- [Online Voice Chatroom](https://github.com/AgoraIO-Usecase/Chatroom)
- [Murder Mystery Game](https://github.com/AgoraIO-Usecase/Murder-Mystery-Game)

## Key Properties

| Property                                          | Agora Live Broadcast Specifications                          |
| ------------------------------------------------- | ------------------------------------------------------------ |
| SDK Package Size                                  | 3.14 MB to 5.88 MB                                              |
| Host Capacity                                     | 17 users                                                  |
| Audience Capacity                                 | One million users                                       |
| Audio Profile                                     | <li>Sample rate: 16 kHz to 48 kHz.<li>Support for mono and stereo sound  |
| Audio Anti-packet-loss Rate                       | 70% (uplink and downlink)                               |
| Latency between the Host and the Host-in Audience/Host | 200 ms to 600 ms                                                  |

## Compatibility

The Agora Native SDK for Interactive Broadcast is supported on platforms such as iOS, Android, Windows, macOS, Electron, Unity, and Web, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

| Platform             | Supported Version                                            |
| -------------------- | ------------------------------------------------------------ |
| Android              | 4.1+<br>The Android SDK supports the following ABIs:<li>armeabi-v7a<li>arm64-v8a<li>x86<li>x86_64 |
| iOS                  | 8.0+                                                         |
| Windows              | Windows 7+<br>The Windows SDK supports the following architecture:<li>x86<li>x64                                                      |
| macOS                | 10.0+                                                        |
| Unity                | <p>2017+</p><p>The Unity SDK supports the following platforms:<p><ul><li>Android (armeabi-v7a, arm64-v8a, x86)<li>iOS<li>Windows (x86, x86_64)<li>macOS                                                        |
| Web                  | <li>Chrome 58+<li>Chrome 49 on Windows XP<li>Firefox 56+<li>Safari 11+<li>Opera 45+<li>QQ 10+<li>360 Security Browser 9.1+ |

<div class="alert note">The browser support on the Web platform varies with the device and system. See <a href="https://docs.agora.io/cn/faq/browser_support">Which browsers does the Agora Web SDK support?</a></div>

## Reference

[How many users can Agora RTC SDK support at the same time?](https://docs.agora.io/en/faq/capacity)
