
---
title: Release Notes
description: 
platform: Cocos
updatedAt: Wed Sep 16 2020 09:26:42 GMT+0800 (CST)
---
# Release Notes
## Introduction

The Agora Interactive Gaming Voice SDK provides the voice function for gaming. For the key features inlcudes, see [Interactive Gaming Overview](https://docs.agora.io/en/Interactive%20Gaming/product_gaming?platform=All%20Platforms).

## v2.1.0

The version 2.1.0 was released on February 27, 2018. See below for new features, improvements, and issues fixed.


### New Features

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Function</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Indicate a Role Change</td>
<td>Added a callback interface in the command mode to indicate a role change between the host and audience in the channel.</td>
</tr>
<tr><td>Updated the API Names</td>
<td>Deleted <code>ForGaming</code> in all the API classes and enumerations.</td>
</tr>
</tbody>
</table>



### Improvements

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Improvement</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Reduced the Bandwidth</td>
<td>Before v2.1.0: If you muted the audio stream of the remote user, the network still sent the stream. Starting from v2.1.0: If you mute the audio stream of the remote user, the network will not send the stream to reduce the bandwidth.</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasional crashes under specific circumstances.
-   Unexpected behavior related to the audio routing.


## v2.0

The version 2.0 was released on August 26, 2017. See below for new features and issues fixed.

### New Features

Added a function of adjusting the background music with the volume keys after the audio is muted.

### Issues Fixed

-   Audio playback issues on specific devices.
-   Abnormal recording on specific devices.


## v1.1

The version 1.1 was released on May 25, 2017. 

- Native-iOS: Renamed the <code>joinChannel</code> method to <code>joinChannelByToken</code>.
- Native-iOS: Added the <code>startAudioRecording</code> and <code>stopAudioRecording</code> methods. 
- Native-iOS: Added the <code>onWarning</code> message to indicate low recording volume.
- Native-Android: Added the <code>startAudioRecording</code> and <code>stopAudioRecording</code> methods.
- Android: Optimized the native `pause`/`resume` interfaces.
- Optimized the audio quality on certain cell phones.
- Fixed the recording noise when calling <code>startAudioRecording</code>.



## v1.0

The version 1.0 was released on May 3, 2017.

This is the first release of the SDK, which includes the following functions:

### 1. Provided the following APIs:


- Unity3D SDK (C# APIs).
- Cocos2d SDK (C++ APIs).
- Native SDK: Android (Java APIs) and iOS (Objective-C APIs).
- Multiple audio effect mixing, such as preload mode and panning.
- Virtual surround sound panning of each uid’s voice according to the in-game coordinates.
- Voice morphing feature.
- Noise block API: Block all non-vocal sounds to ensure a clear in-game chat.


### 2. Package size reduction.

### 3. Performance optimization.




