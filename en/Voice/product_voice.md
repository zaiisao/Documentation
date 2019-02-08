
---
title: Agora Voice Overview
description: 
platform: All Platforms
updatedAt: Fri Feb 08 2019 07:33:46 GMT+0000 (UTC)
---
# Agora Voice Overview
The Agora Native SDK for Voice Call enables easy and convenient one-to-one or one-to-many **voice-only** calls. With a small SDK package size, the Agora Native SDK for Voice Call is applicable to a variety of recreational and business activities.

The difference between an Agora Voice Call and [Agora Voice Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms) is: 
* An Agora Voice Call prioritizes fluency and low latency. All users are the same role and can talk to each other freely. A typical scenario of an Agora Voice Call is a voice conference call for many persons.
* An Agora Voice Interactive Broadcast prioritizes high voice quality. Users can be the host or audience, where only the host can talk. A user who wants to talk must change the role to a host. A typical scenario of the Agora Voice Interactive Broadcast is an online trivia game.

## Functions and Scenarios

The Agora Native SDK for Voice Call boasts a flexible combination of functions for different scenarios.

| Function                              | Description                                                  | Scenario                                                     |
| ----------------- | ------------------------------------------------------------ | --------------------------------------- |
| Audio Mixing                          | Sends the local and online audio with the user's voice to other audience members in the channel. | <li>Online KTV. <li>Interactive music classes.    |
| Play the Sound Effect Files          | Enables developers to play specific sound effect files, adjust the volume, and set the playback position of the sound effect files.        | Online chess or card games.                                |
| Adjust the Pitch     | Adjusts the pitch and uses the equalization and reverberation modes.                    | <li>Online KTV.<li>To change the voice in an online chatroom.        |
| Enable Two-channel/High-quality Sound Mode | Enables the two-channel and the high-quality sound mode.                               | <li>Online music classes.<li> FM applications.        |
| Modify the Raw Data                    | Enables developers to obtain and modify the raw voice data to create special effects, such as a voice change. | To change the voice in an online voice chatroom. |

## Key Properties

| Property                                          | Agora Voice Call Specifications                          |
| ------------ | ------------------------------------------------------------ |
| SDK Package Size                                  | 3.69 MB to 7.75 MB                                              |
| Capacity     | Seven users (To support more users, use [Agora Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms)) |
| Audio Profile                                     | <li>Sample rate: 16 KHz to 48 KHz<li>Support for mono and stereo sound |
| Audio Anti-packet-loss Rate                       | 70% (uplink and downlink)                               |

## Compatibility

The Agora Native SDK for Voice Call is supported on platforms such as iOS, Android, Windows, macOS, Linux, Web, and WeChat mini-programs, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

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
