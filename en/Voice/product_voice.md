
---
title: Agora Voice Overview
description: 
platform: All Platforms
updatedAt: Tue Jun 30 2020 02:08:31 GMT+0800 (CST)
---
# Agora Voice Overview
The Agora Native SDK for Voice Call enables easy and convenient one-to-one or one-to-many **voice-only** calls. With a small SDK package size, the Agora Native SDK for Voice Call is applicable to a variety of recreational and business activities.

The difference between an Agora Voice Call and [Agora Voice Interactive Broadcast](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All%20Platforms) is: 
* An Agora Voice Call prioritizes smoothness and low latency. All users are the same role and can talk to each other freely. A typical scenario of an Agora Voice Call is a voice conference call for many persons.
* An Agora Voice Interactive Broadcast prioritizes high voice quality. Users can be the host or audience, where only the host can talk. A user who wants to talk must change the role to a host. A typical scenario of the Agora Voice Interactive Broadcast is an online trivia game.

## Functions and Scenarios

The Agora Native SDK for Voice Call boasts a flexible combination of functions for different scenarios.

<style> table th:first-of-type {     width: 150px; } th:third-of-type {     width: 170px; }</style>

| Function                              | Description                                                  | Scenario                                                     |
| ----------------- | ------------------------------------------------------------ | --------------------------------------- |
| Audio mixing                          | Sends the local and online audio with the user's voice to other audience members in the channel. | <li>Online KTV. <li>Interactive music classes.    |
| Play the sound effect files          | Enables developers to play specific sound effect files, adjust the volume, and set the playback position of the sound effect files.        | Online chess or card games.                                |
| Voice changer and reverberation     | Provides multiple presets to easily change the voice and set reverberation effects, also supports adjusting the pitch and using the equalization and reverberation modes flexibly.                    | <li>Online KTV.<li>To change the voice in an online chatroom.        |
|Spatial Sound Effects |	Sets the spatial sound effects for remote users to provide immersive experiences.|	FPS games. |
| Enable Two-channel/High-quality Sound Mode | Enables the two-channel and the high-quality sound mode.                               | <li>Online music classes.<li> FM applications.        |
| Modify the Raw Data                    | Enables developers to obtain and modify the raw voice data to create special effects, such as a voice change. | To change the voice in an online voice chatroom. |

## Key Properties

| Property                                          | Agora Voice Call Specifications                          |
| ------------ | ------------------------------------------------------------ |
| SDK Package Size                                  | 3.14 MB to 5.88 MB                                              |
| Audio Profile                                     | <li>Sample rate: 16 kHz to 48 kHz<li>Support for mono and stereo sound |
| Audio Anti-packet-loss Rate                       | 70% (uplink and downlink)                               |

## Compatibility

The Agora Native SDK for Voice Call is supported on platforms such as iOS, Android, Windows, macOS, Electron, Unity, and Web, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

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
