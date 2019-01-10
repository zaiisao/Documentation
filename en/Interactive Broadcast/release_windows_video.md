
---
title: Release Notes
description: 
platform: Windows
updatedAt: Thu Jan 10 2019 05:22:59 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Video SDK.

## Overview

The Agora Video SDK for Windows supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

> The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version earlier than v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

### Notice on Windows SDK 2.0

Developers,

Due to the upgrade of Agora products, Windows SDK 2.0 no longer supports the Agora Recording Server v1.8.2 or earlier, or relevant APIs.

This may affect:

-   Users of both the Windows SDK and Agora Recording Server v1.8.2 or earlier.
-   The following API methods are no longer supported: startRecordingService, stopRecordingService, and refreshRecordingServiceStatus.


Two solutions are available:

1.  Migrate from the Agora Recording Server v1.8.2 or earlier to the Agora Recording SDK v1.12 or later. The Agora Recording SDK does not require recording on the client side and does not affect subsequent upgrades of the Windows SDK (recommended). For how to use the Agora Recording SDK, refer to:

    -   [Quickstart Guides](../../en/API%20Reference/recording_cpp-1.md)
    -   [API Reference](../../en/API%20Reference/recording_cpp.md)

2.  For users who want to continue using the Agora Recording Server, you can use your current Windows SDK version (v1.14 or earlier). Contact Technical Support for any questions.

## v2.2.1

v2.2.0 was released on May 30, 2018, and improves the internal code implementation. 

## v2.2.0

v2.2.0 was released on May 4, 2018. 

### New Features

#### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method for a remote user in a channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from an earlier version, pay attention to the functional changes of this method.

#### 2. Deploy the proxy at the server

Agora provides a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. For details, see [Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy.md).

#### 3. Get the remote video state

Adds the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

#### 4. Add watermarks on the broadcasting video

Adds the watermark function for users to add a PNG file to a local or CDN live broadcast as a watermark. Adds the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add (and remove) watermarks in a local live-broadcast. The <code>watermark</code> parameter in the <code>LiveTranscording</code> interface enables the watermark in a CDN live broadcast. 

### Improvements

#### 1. Audio volume indication

Improves the function of <code>enableAudioVolumeIndication</code>. The method, once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in a channel, the <code>onNetworkQuality</code> method improved its data accuracy. 

#### 3. Last-mile network quality detection before joining a channel

To test if the customers’ network condition can support audio or video calls before joining the channel, the <code>onLastmileQuality</code> callback has changed its detection base from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, the SDK still triggers this callback once every two seconds. 

#### 4. Audio quality enhancement

Improves the audio quality in music playback scenarios.

### Issues Fixed

-   Occasional screen display abnormalities when a large number of the audience joins as hosts in a live-broadcast channel.
-   Occasional audio block issues during a live broadcast.


## v2.1.3

v2.1.3 was released on April 19, 2018. 

In v2.1.3, Agora updates the bitrate values in the <code>setVideoProfile</code> method in the Live-broadcast profile. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

Occasional recording failures on some phones when a user leaves a channel and turns on the built-in recording device.

### Improvements

Improved the performance of screen sharing by shortening the time interval between which users switch from screen sharing to normal communication or live-broadcast.

## v2.1.2

v2.1.2 was released on April 2, 2018. 

>  If you upgraded the SDK to v2.1.2 from an earlier version, the live-broadcast video quality is better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth.

### Issues Fixed

-   The co-host is not seen by the other host after leaving and rejoining a channel.
-   The video resolution of a shared screen is worse in the communication profile than in the live-broadcast profile.


## v2.1.1

v2.1.1 was released on March 16, 2018. 

Agora identified a critical issue in v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0

v2.1.0 was released on March 7, 2018. See the following sections for new features, improvements, and issues fixed.

### New Features

#### 1. Voice optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhanced audio effect input from a built-in microphone

In an interactive broadcast scenario, the host can enhance the local audio effects from a built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods to implement the expected voice equalization and reverberation effects.

#### 3. Online statistics query

Adds Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host. See:

-   In a voice or video call scenario, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
-   In an interactive broadcast scenario, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).


#### 4. 17-way video

Adds the support of 17-way video in an interactive broadcast scenario, see:

-   [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_ios.md)
-   [Video Conference of 7+ Users](../../en/Interactive%20Broadcast/seventeen_people_windows.md)


#### 5. Injecting an external video stream

Adds the function of injecting an external video stream to an ongoing live broadcast, see [Injecting an External Stream to a Live Broadcast](../../en/Quickstart%20Guide/inject_stream_ios.md).


#### 6. Screen sharing for interactive broadcast

-   Before v2.1.0: The Agora SDK supported the screen-sharing function only in video calls.
-   From v2.1.0: The Agora SDK supports the screen-sharing function in video-calls and interactive-broadcasts.


#### 7. Loopback recording

Adds the <code>enableLoopbackRecording</code> method to collect all local sounds.

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
<tr><td>Video Freeze Rate</td>
<td>Reduces the video freeze rate in the audience mode and for specific devices.</td>
</tr>
<tr><td>Authentication</td>
<td>Supports a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each token in the new authentication mechanism includes all privileges (for example, joining a channel, host-in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions are billed according to the voice-only mode. For example, 16 &times; 16.</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasionally the camera cannot capture the video source.
-   Occasional video freeze issues.
-   Occasional crashes.


## v2.0 and Earlier
### v2.0

v2.0 was released on November 21, 2017. 

#### New Features

-   Adds the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the communication profile to support dual streams.
-   Supports the external audio source in the communication and live broadcast profiles by adding the following API methods:

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
<tr><td><code>setExternalAudioSource</code></td>
<td>Sets the external audio source parameters, including the sample rate and the number of channels.</td>
</tr>
<tr><td><code>pushExternalAudioFrame</code></td>
<td>Pushes an external audio frame to the Agora SDK.</td>
</tr>
</tbody>
</table>



-   Provides a set of RESTful APIs to ban a peer user from the server in the communication and live broadcast profiles. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function, if required.
-   Supports Windows (x64).
-   Adds API method to control the volume, mute, and system volume settings.

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
<tr><td><code>setPlaybackDeviceVolume</code></td>
<td>Sets the volume of the playback device.</td>
</tr>
<tr><td><code>getPlaybackDeviceVolume</code></td>
<td>Gets the volume of the playback device.</td>
</tr>
<tr><td><code>setRecordingDeviceVolume</code></td>
<td>Sets the volume of the microphone.</td>
</tr>
<tr><td><code>getRecordingDeviceVolume</code></td>
<td>Gets the volume of the microphone.</td>
</tr>
<tr><td><code>setPlaybackDeviceMute</code></td>
<td>Mutes the playback device.</td>
</tr>
<tr><td><code>getPlaybackDeviceMute</code></td>
<td>Gets the mute status of the playback device.</td>
</tr>
<tr><td><code>setRecordingDeviceMute</code></td>
<td>Mutes the microphone.</td>
</tr>
<tr><td><code>getRecordingDeviceMute</code></td>
<td>Gets the mute status of the microphone.</td>
</tr>
<tr><td><code>onAudioDeviceVolumeChanged</code></td>
<td>Notifies that the volume of the playback device, recording device, or application has changed.</td>
</tr>
</tbody>
</table>




### v1.14

v1.14 was released on October 20, 2017. 

#### New Features

-   Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch.

-   Live Broadcast: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimizes the audio at high bitrates.
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (226 ms on average, and 204 ms under good network conditions).
-   Adds the ability to reduce the bandwidth:
    -   Before v1.14: If you muted the audio or disabled the video of a specific user, the network still sent the streams.
    -   Starting from v1.14: If you mute the audio or disable the video of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was in poor conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


#### Issues Fixed:

-   Unable to hear any audio on certain Windows systems.
-   Camera-related issues on certain Windows systems.


### v1.13.1

v1.13.1 was released on September 28, 2017. 

This version optimizes the echo issue under certain circumstances.

### v1.13

v1.13 was released on September 4, 2017. See the following sections for new features, improvements, and issues fixed.

#### New Features:

-   Adds the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Adds the function to disable the audio playback.
-   Supports the profile configuration for stream-pushing on the client side.
-   Adds the <code>onClientRoleChanged</code> callback to indicate a user role change between a host and an audience in a live broadcast.
-   Supports the push-stream failure callback on the server side.


#### Improvements:

-   Screen Sharing: Enhances the video definition and fluency.
-   Screen Sharing: Supports updating the captured region dynamically.
-   The video profile is controllable by the software codec.


#### Issues Fixed:

Occasional crashes.

### v1.12

v1.12  was released on July 25, 2017. 

#### New Features:

-   Adds the <code>injectStream</code> method to inject an RTMP stream into the current channel in a live broadcast scenario.
-   Adds the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code> method.
-   Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Adds a set of API methods to manage the audio effect.
-   Adds the <code>ActiveSpeaker</code> method to indicate who is the active speaker in the current channel.
-   Removes the <code>setScreenCaptureWindow</code> method and updates the <code>startScreenCapture</code> method to share the whole screen and specify the window or region in the communication profile.
-   Adds displaying the mouse function when the screen-sharing function is enabled in the communication profile.
-   Web: Adds and updates a series of API methods to enable interoperability between Web browsers and native clients for Communication or Live-broadcast profiles. See [Release Notes](../../en/Interactive%20Broadcast/release_web_video.md).


Recording: Adds real-time video mixing, web recording, and callbacks. See [Release Notes for the Recording SDK](../../en/Product%20Overview/release_recording.md).

#### Improvements:

Android/iOS/macOS/Windows: In the Communication profile, an improvement for the 320 &times; 180 resolution profile:

- Keeps the video smooth under poor network and equipment conditions.
- Enhances the image quality to be better than 180p under good network and equipment conditions.



#### Issues Fixed:

- Android: Bluetooth issues related to audio routing.
- Android/iOS/macOS/Windows: Occasional crashes.







