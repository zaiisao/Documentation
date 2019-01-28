
---
title: Release Notes
description: 
platform: Unity
updatedAt: Mon Jan 28 2019 11:27:29 GMT+0000 (UTC)
---
# Release Notes
## Introduction

The Agora Interactive Gaming Voice SDK provides the voice function for gaming. For the key features, see [Interactive Gaming Overview](https://docs.agora.io/en/Interactive%20Gaming/product_gaming?platform=All%20Platforms).

## v2.1.0

v2.1.0 was released on February 27, 2018. 

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
<tr><td>Set the Voice-only Mode</td>
<td>Adds an interface to set the voice-only mode to optimize the audio quality.</td>
</tr>
<tr><td>Set the Audio Effect Position</td>
<td>Adds an interface to set the position and volume of the audio effects sent from the remote user.</td>
</tr>
<tr><td>Set the Local Voice Pitch</td>
<td>Adds an interface to set the pitch of the local voice.</td>
</tr>
<tr><td>Indicate a Role Change</td>
<td>Adds a callback in the command mode to indicate a user role change between a host and an audience in a channel.</td>
</tr>
<tr><td>Indicate the Video is Muted</td>
<td>Adds a callback to indicate whether a remote user muted/unmuted the video stream.</td>
</tr>
<tr><td>Update the API Names</td>
<td>Deletes <code>ForGaming</code> in all API classes and enumerations.</td>
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
<tr><td>Reduce the Bandwidth</td>
<td>
<ul>
<li>Before v2.1.0: If you muted the audio or video stream of the remote user, the network still sent the stream.</li> 
<li>Starting from v2.1.0: If you mute the stream of the remote user, the network will not send the stream to reduce the bandwidth.</li>
</ul></td>
</tr>
</tbody>
</table>



### Fixed Issues

-   Occasional crashes under specific circumstances.
-   Unexpected behavior related to the audio routing.


## v2.0

v2.0 was released on August 26, 2017. 

### New Features:

-   Adds the video functions for Unity.
-   Adds a function of adjusting the background music with the volume keys after the audio is muted.


### Fixed Issues:

-   Audio effect playback-related issues.
-   Low-volume with earphones on iOS devices.
-   Recording-related issues on Android devices.


## v1.1

v1.1 was released on May 25, 2017. 

- Native-iOS: Renames the <code>joinChannel</code> method to the <code>joinChannelByToken</code> method.
- Native-iOS: Adds the <code>startAudioRecording</code>and <code>stopAudioRecording</code> methods.
- Native-iOS: Adds the <code>onWarning</code> message to indicate low-recording volume.
- Native-Android: Adds the <code>startAudioRecording</code> and <code>stopAudioRecording</code> methods.
- Android: Optimizes the native <code>pause</code>/<code>resume</code> methods.
- Optimizes the audio quality on certain cell phones.
- Fixes the recording noise when calling the <code>startAudioRecording</code> method.


## v1.0 

v1.0 was released on May 3, 2017.

This is the first release of the SDK, which includes the following functions:

### 1. Provided the following API methods:

- Unity3D SDK (C# APIs).
- Cocos2d SDK (C++ APIs).
- Native SDK: Android (Java APIs) and iOS (Objective-C APIs).
- Multiple audio effect mixing, such as preload mode and panning.
- Virtual surround-sound panning of each UID’s voice according to the in-game coordinates.
- Voice morphing feature.
- Noise block API: Block all non-vocal sounds to ensure a clear in-game chat.

### 2. Package size reduction.

### 3. Performance optimization.
