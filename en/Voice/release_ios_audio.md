
---
title: Release Notes
description: 
platform: iOS
updatedAt: Fri Nov 23 2018 08:39:54 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Voice SDK for iOS.

## Overview

The Voice SDK supports the following scenarios:

-   Voice Communication
-   Live Voice Broadcast

## v2.3.1

The version 2.3.1 was released on September 28, 2018. See below for new features, improvements, and issues fixed.

### New Features

#### Disables/Re-Enables the Local Audio Function

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio stream after joining a channel, this release adds the `enableLocalAudio` method to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK returns the `didMicrophoneEnabled` callback function, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.

The difference between this method and `muteLocalAudioStream` is that `enableLocalAudio` does not capture or send any audio stream, while `muteLocalAudioStream`captures but does not send audio streams.

### Improvements

-  Reduced the CPU consumption in the audio communication profile on some low-end iOS devices.

### Issues Fixed

- Occasional crashes on some iOS devices
- In the live broadcast profile, delay at the client due to incorrect statistics.


## v2.3.0

The version 2.3.0 was released on August 31, 2018. See below for new features, improvements, issues fixed and API Changes.

### Before Reading

-   An Accelerate.framework library was added to the SDK in v2.3.0, which is capable of large-scale mathematical computations and image calculations, optimized for high performance.
-   The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).


### New Features

#### 1. Notifies the user that the Token will expire in 30 seconds

The SDK returns the `tokenPrivilegeWillExpire` callback function 30 seconds before a Token expires to notify the app to renew it. When this callback function is received, you need to generate a new Token on your server and call `renewToken` to pass the newly-generated Token to the SDK.

#### 2. Returns user-specific upstream and downstream statistics, including the bitrate, frame rate, packet loss rate, and time delay

The `audioTransportStatsOfUid` callback function is added to provide user-specific upstream and downstream statistics, including the bitrate, frame rate, and packet loss rate. During a call or a live broadcast, this callback function is triggered once every two seconds after the user receives audio packets from a remote user. The callback includes the user ID, audio bitrate at the receiver, packet loss rate, and time delay (ms).

#### 3. Sets the SDK’s control over an audio session

The SDK and app both have control over the audio session. However, the app may restrict the SDK’s control over an audio session and allow another app or a third-party component to control it by using the `setAudioSessionOperationRestriction` method. You can implement different levels of control by choosing the corresponding restriction. You can call this method before or after joining the channel.

### Improvements

-   Improved the quality for one-on-one voice/video scenarios with optimized latency and smoothness, especially for areas like Southeast Asia, South America, Africa, and the Middle East.
-   Improved the audio encoder efficiency in a live broadcast to reduce user traffic while ensuring the call quality.
-   Improved the audio quality during a call or a live broadcast using the deep-learning algorithm.


### Issues Fixed

- Crashes after publishing streams from some iOS devices.
- Occasional crashes on some iOS devices.
- Crashes when a user frequently mutes and resumes all sound effects on some iOS devices.
- Excessive increase in the memory usage for the host when the host frequently joins and leaves a channel that has multiple delegated hosts.
- Occasionally, the remote user cannot hear the host when the host switches between the AUDIENCE and BROADCASTER.
- Occasionally on some iOS devices, a user fails to hear any sound after returning to the channel from a system phone call.
- Occasionally, the audience cannot adjust the channel volume.
- Occasional crashes when a user frequently joins and leaves the channel.
- Occasional crashes when one of the two broadcasters mutes or disables the local audio while playing the background music.
- Occasional crashes on iOS devices when the device interoperates with the Web and when a Web user frequently joins and leaves a channel.
- Occasional crashes on some devices when preloading the sound effects.
- Occasional inter-operational failures between an iOS and a macOS device.
- Occasional crashes on some iOS devices when a user leaves the live broadcast channel while playing music using a third-party application.
- Occasional crashes on some iOS devices when leaving the channel.
- Occasional echoes when using a specific audio card.
- Failure to adjust the volume on some iOS devices.


### API Changes

To improve the user experience, Agora has made the following changes to the APIs:

To avoid adding too many users with the same uid into the CDN publishing channel, the following API methods are added in v2.3.0:

-   `addUser`
-   `removeUser`


The following API methods are deleted and no longer supported in v2.3.0. Agora provides the Recording SDK for better recording services. For more information on the Recording SDK, see [Release Notes for Agora Recording SDK](../../en/Product%20Overview/release_recording.md).

-   <code>startRecordingService</code>
-   <code>stopRecordingService</code>
-   <code>refreshRecordingServiceStatus</code>


The following deprecated API method is deleted and no longer supported from v2.3.0:

-   <code>setSpeakerphoneVolume</code>


### Backwards Compatibility Issues

None.

### Known Issues and Limitations

-   To use the <code>startAudioMixing</code> method, ensure that the iOS device is 8.0+.
-   Encryption does not interoperate with the Web SDK (based on Web RTC).
-   Bluetooth on iPhone 8 is not supported.


## v2.2.3 

The version 2.2.3 was released on July 5, 2018. See below for issues fixed.

### Read This First

The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

### Issues Fixed

- Occasional online statistics crashes.
- Occasional crashes during a live broadcast.
- Excessive increase in the memory usage when multiple delegated hosts broadcast in the channel.
- Failing to report the uid and volume of the speaker in a channel.
- Unsteady voice volume of the broadcaster in a live broadcast.


## v2.2.1

The version 2.2.1 was released on May 30, 2018. See below for issues fixed.

### Issues Fixed

- Occasional crashes on some iOS devices.
- Occasional memory leak on some iOS devices.
- Occasional app crashes when the app starts audio mixing on some iOS devices.


## v2.2.0

The version 2.2.0 was released on May 4, 2018. See below for new features and improvements.

### New Features

#### 1. Play the audio effect in the channel

Added a <code>publish</code> parameter in the <code>playEffect</code> method, to enable the remote user in the channel to hear the audio effect played locally. 

> If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the proxy at the server

Agora has provided a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. For details, see [Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy.md).

### Improvements

#### 1. Audio volume indication

Improved the function of <code>enableAudioVolumeIndication</code>. This method once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method has improved its data accuracy. 

#### 3. Last mile network quality detection before joining a channel

To test if the customers’ network condition can support voice or video calls before joining the channel, <code>onLastmileQuality</code> changed the detection from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, this callback function is still triggered at two-second intervals. 

#### 4. Audio quality enhancement

Improved the audio quality in scenarios that involve music playback. To achieve high-fidelity music playback, you can set <code>Scenario：AgoraAudioScenarioGameStreaming = 3</code> in the <code>setAudioProfile</code> method. 
#### 5. Bitcode support

Added support for Bitcode, which enables App optimization and thinning by the App Store. The package size of the Bitcode SDK is 2.5 times that of the normal one.

### Issues Fixed

-   Fixed occasional echo issues caused by some iOS device.
-   Fixed occasional screen display abnormalities when a large number of the audience join as the host in the live-broadcast channel.


## v2.1.3

The version 2.1.3 was released on April 19, 2018. See below for issues fixed.

### Issues Fixed

-   Block callbacks were occasionally not received if the delegate was not set.
-   NSAssertionHandler appeared in external links to the SDK.
-   Occasional recording failures on some phones when the user leaves the channel and turns on the built-in recording device.
-   Occasional crashes in a communication or live-broadcast session.


## v2.1.2

The version 2.1.2 was released on April 2, 2018. See below for issues fixed.

### Issues Fixed

Video freeze in DTX + AAC mode.

## v2.1.1

The version 2.1.1 was released on March 16, 2018. 

Agora has identified a critical issue in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0 

The version 2.1.0 was released on March 7, 2018. See below for new features, improvements, and issues fixed.

### New Featurens

#### 1. Voice Optimization

Added a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhanced audio effect input from a built-in microphone

In an interactive broadcast, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online statistics query

Added Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

-   For voice or video calls see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
-   For interactive broadcasts, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).


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
<tr><td>Authentication</td>
<td>Supported a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each Token in the new authentication mechanism includes all the privileges (for example, joining a channel, hosting in, and stream-pushing).</td>
</tr>
<tr><td>API Naming Optimization</td>
<td>Modified a set of names for the API attributes and enumeration values. </a>.</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasional crashes.
-   Occasionally no voice after turning off the microphone.


## v2.0.2

The version 2.0.2 was released on December 15, 2017. See below for issues fixed.

Fixed the FFmpeg symbol conflict.

## v2.0

The version 2.0 was released on December 6, 2017. See below for new features and issues fixed.

### New Features

- Updated the following callback functions for audio mixing and sound effects:


  <table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</strong></th>
<th><strong>Description</strong></th>
</tr>
</thead>
<tbody>
<tr><td><code>rtcEngineMediaEngineDidAudioMixingFinish</code></td>
<td>Removed. Replaced by <code>rtcEngineLocalAudioMixingDidFinish</code>.</td>
</tr>
<tr><td><code>rtcEngineDidAudioEffectFinish</code></td>
<td>Added. Notifies that the local audio effect has stopped.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidStart</code></td>
<td>Added. Notifies that the remote has started the audio mixing.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidFinish</code></td>
<td>Added. Notifies that the remote has stopped the audio mixing.</td>
</tr>
</tbody>
</table>



- Supported the external audio source in the communication and live broadcast scenarios by adding the following API methods:

   <table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>enableExternalAudioSourceWithSampleRate</code></td>
<td>Enables the external audio source function.</td>
</tr>
<tr><td><code>disableExternalAudioSource</code></td>
<td>Disables the external audio source function.</td>
</tr>
<tr><td><code>pushExternalAudioFrameRawData</code></td>
<td>Pushes the external audio frame.</td>
</tr>
</tbody>
</table>



-   Provided a set of RESTful APIs to ban a peer user from the server in the communication and live broadcast scenarios. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function if required.


### Issues Fixed

Audio routing and Bluetooth issues.

## v1.14 

### New Functions

The version 1.14 was released on October 20, 2017. See below for new features and improvements.

-   Added the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Added the <code>setLocalVoicePitch</code> method to set the local voice pitch.
-   Live Broadcast: Added the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.

### Improvements

-   Optimized the audio at high bitrates.
-   Added the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.


### Issues Fixed:

Occasional crashes on iOS devices.

## v1.13.1

The version 1.13.1 was released on September 28, 2017. 

-   Fixed the issue of not able to adjust the volume in the speaker mode on iOS 11 with iPhone 7+.
-   Optimized the echo issue.


## v1.13

The version 1.13 was released on September 4, 2017. See below for new features, improvements, and issues fixed.

-   Added the <code>didClientRoleChanged</code> callback function to indicate a user role change between the host and audience in a live broadcast scenario.
-   Fixed occasional crashes.


## v1.12

The version 1.12  was released on July 25, 2017. See below for new features and issues fixed.

### New Features:

-   Added the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code>  method.
-   Added the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Added the following API methods to manage the audio effects:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>API</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>getEffectsVolume</code></td>
<td>Gets the volume of the audio effects from 0.0 to 1.0.</td>
</tr>
<tr><td><code>setEffectsVolume</code></td>
<td>Sets the volume of the audio effects.</td>
</tr>
<tr><td><code>setVolumeOfEffect</code></td>
<td>Adjusts the volume of the specified sound effect in real-time.</td>
</tr>
<tr><td><code>playEffect</code></td>
<td>Plays the audio effect.</td>
</tr>
<tr><td><code>stopEffect</code></td>
<td>Stops playing a specific audio effect.</td>
</tr>
<tr><td><code>stopAllEffects</code></td>
<td>Stops playing all audio effects.</td>
</tr>
<tr><td><code>preloadEffect</code></td>
<td>Preloads a specific audio-effect file (compressed audio file) to the memory.</td>
</tr>
<tr><td><code>unloadEffect</code></td>
<td>Releases the specific preloaded audio effect from the memory.</td>
</tr>
<tr><td><code>pauseEffect</code></td>
<td>Pauses the specific audio effect.</td>
</tr>
<tr><td><code>pauseAllEffects</code></td>
<td>Pauses all audio effects.</td>
</tr>
<tr><td><code>resumeEffect</code></td>
<td>Resumes playing a specific audio effect.</td>
</tr>
<tr><td><code>resumeAllEffects</code></td>
<td>Resumes all audio effects.</td>
</tr>
</tbody>
</table>




### Issues Fixed:

Occasional crashes.




