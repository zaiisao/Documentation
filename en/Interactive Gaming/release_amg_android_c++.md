
---
title: Release Notes
description: 
platform: Android
updatedAt: Mon Jan 28 2019 11:27:41 GMT+0800 (CST)
---
# Release Notes
## Introduction
The Agora Android SDK for Interactive Gaming provides Java and C++ Interfaces to implement the voice and video functions for gaming on the Android platform. 

For key features, see [Interactive Gaming Overview](https://docs.agora.io/en/Interactive%20Gaming/product_gaming?platform=All%20Platforms).

## v2.2

Developed on the basis of the Agora Native SDK and Agora C++ SDK, v2.2 was released on January 28, 2019. 

### New Features

#### 1. Set the Voice-only Mode

Adds the `setVoiceOnlyMode` method to set the voice-only mode.  Once voice-only mode is enabled, the SDK transmits only the voice stream, but not other streams, like the sound of keyboard strokes. This function effectively optimizes the audio quality.

#### 2. Set the Audio Effect Playback Position

Adds the `setRemoteVoicePosition` method to set the playback position and volume of the audio effects sent from the remote user.

#### 3. Disables/Re-enables the Local Audio Function

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio stream after joining a channel, this version adds the `enableLocalAudio` method to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK triggers the `onMicrophoneEnabled` callback, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.
