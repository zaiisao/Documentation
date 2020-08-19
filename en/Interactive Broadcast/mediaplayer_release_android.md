
---
title: Release Notes
description: draft
platform: Android
updatedAt: Wed Aug 19 2020 11:12:13 GMT+0800 (CST)
---
# Release Notes
This page provides release notes for the Agora MediaPlayer Kit plugin.

## Overview
The Agora MediaPlayer Kit is a media player plug-in developed for live interactive streaming scenes, and is compatible with the Agora Native SDK (v2.4.0 or later).

This plug-in helps developers enable the function of playing media resources in real-time live interactive streaming through the use of streamlined and flexible APIs, and synchronously sharing local or online media resources played by the host to all users in the channel. See [function description](https://docs.agora.io/en/Interactive%20Broadcast/mediaplayer_android?platform=Android#function-description) for details.

To enrich the live interactive streaming playability and improve the real-time interactive experience, we recommend using the mediaplayer kit in the following scenarios:
- Local playback: Play local or online media resources.
- Online education: The teacher shares a currently playing video with the students during an online class.
- Live sports: The host shares the live sports with the audience during his/her live interactive streaming.
- Pseudo live interactive streaming: Share or publish the video recorded by the host in advance to the audience.

## v1.1.4

v1.1.4 was released on August 19, 2020.

Improvements and fixed issues are as follows:
- Reduced the performance loss of the device during playback.
- Reduced the size of the MediaPlayer Kit package.
- Fixed occasional crashes and application unresponsiveness issues.

## v1.1.2

v1.1.2 was released on Jun 15, 2020.

New features and improvements are as follows:
- Added support for the X86_64 architecture.
- Added support for using MediaPlayer Kit C++ APIs.
- Optimized the structure of the MediaPlayer Kit package.

## v1.1.1

v1.1.1 was released on May 11, 2020.

This release fixed errors that occur when you play some special video files.

## v1.1.0

v1.1.0 was released on Feb 28, 2020.

This is the first release of the mediaplayer kit. You can use it in your project to enable the following functions:

#### 1. Sharing media resources
To enrich the live playability, the host can synchronously share the playback of the local and online media resource with all users in the channel.

#### 2. Playing multiple media resources simultaneously
To meet various demands for a varied audience, the host can play multiple media resources simultaneously by creating multiple instances of AgoraMediaPlayerKit.

#### 3. Playback controls
The host has access to real-time playback controls for opening the media resource, playing the media resource, pausing the playback, resuming the playback, and seeking to the new playback position of the media resource.

#### 4. Precise volume controls
To precisely control the playback volume at different stages, the hosts can adjust the local and remote playback volume separately, which improves the user experience on both the playback and subscription ends.

#### 5. Getting playback information
The host can actively obtain various playback information, such as current playback progress, playback state, and detailed media stream information.

#### 6. Registering observer and monitoring events
The observer class contains a series of events, such as playback progress, playback state, and the result of a seek operation to a new playback position. By listening for these events, you can have more control over the playback process. When an exception occurs, you can use these event callbacks for troubleshooting.

Besides, you can listen for events that report receiving the media metadata, each audio frame and each video frame. These events help you include more complex functions in multiple scenarios, such as using custom format data, recording audio, recording video, and screenshots.
