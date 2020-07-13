
---
title: Release Notes
description: 
platform: Windows
updatedAt: Fri Jul 10 2020 10:10:51 GMT+0800 (CST)
---
# Release Notes
This page provides release notes for the Agora MediaPlayer Kit.

## Overview

The Agora MediaPlayer Kit is a media player plug-in developed for live interactive streaming scenes, and is compatible with the Agora Native SDK (v2.4.0 or later).

This plug-in helps developers enable the function of playing media resources in real-time live interactive streaming through the use of streamlined and flexible APIs, and synchronously sharing local or online media resources played by the host to all users in the channel. See [function description](https://docs.agora.io/en/Interactive%20Broadcast/mediaplayer_win?platform=Windows#function-description) for details.

To enrich the live interactive streaming playability and improve the real-time interactive experience, we recommend using the mediaplayer kit in the following scenarios:
- Local playback: Play local or online media resources.
- Online education: The teacher shares a currently playing video with the students during an online class.
- Live sports: The host shares the live sports with the audience during his/her live interactive streaming.
- Pseudo live interactive streaming: Share or publish the video recorded by the host in advance to the audience.

## v1.1.1

v1.1.1 was released on May 11, 2020.

This release fixed the following issues:

- Errors occur when you play some video files and RTMP streams.
- Crashes occur on certain Windows versions.

## v1.1.0

v1.1.0 was released on Mar 30, 2020.

<div class="alert note">To improve your experience, this release made important changes to the original methods, so re-integration is needed.</div>

This release enables the following functions:

#### 1. Sharing media resources
To enrich the live playability, the host can synchronously share the playback of the local and online media resource with all users in the channel.

#### 2. Playing multiple media resources simultaneously
To meet various demands for a varied audience, the host can play multiple media resources simultaneously by creating multiple instances of `IMediaPlayer`.

#### 3. Playback controls
The host has access to real-time playback controls for opening the media resource, playing the media resource, pausing the playback, resuming the playback, and seeking to the new playback position of the media resource.

#### 4. Precise volume controls
To precisely control the playback volume at different stages, the hosts can adjust the local and remote playback volume separately, which improves the user experience on both the playback and subscription ends.

#### 5. Getting playback information
The host can actively obtain various playback information, such as current playback progress, playback state, and detailed media stream information.

#### 6. Registering observer and monitoring events
The observer class contains a series of events, such as playback progress, playback state, and the result of a seek operation to a new playback position. By listening for these events, you can have more control over the playback process. When an exception occurs, you can use these event callbacks for troubleshooting.

Besides, you can listen for events that report receiving the media metadata, each audio frame and each video frame. These events help you include more complex functions in multiple scenarios, such as using custom format data, recording audio, recording video, and screenshots.

## v1.0.0

v1.0.0 was released on Sep 4, 2019.

This is the first release of the mediaplayer kit. You can use it in your project to enable the following functions:

#### 1. Sharing media resources

To enrich the live playability, the host can synchronously share the playback of the local and online media resource with all users in the channel.


#### 2. Playback controls

The host has access to real-time playback controls for opening the media resource, playing the media resource, pausing the playback, resuming the playback, and seeking to the new playback position of the media resource.

#### 3. Precise volume controls

To precisely control the playback volume at different stages, the hosts can adjust the local and remote playback volume separately, which improves the user experience on both the playback and subscription ends.

#### 4. Monitoring events

The  `MediaPlayerKitEventHandler` and `MediaInfoCallback` class provide callbacks to monitoring these events:

- The playback state.
- The playback error.
- The audio information.
- The video information.
