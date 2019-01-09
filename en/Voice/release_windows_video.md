
---
title: Release Notes
description: 
platform: Windows
updatedAt: Wed Jan 09 2019 16:59:01 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Video SDK.

## Overview

The Video SDK for Windows supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms) and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

> The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version earlier than v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

### Notice on Windows SDK 2.0

Developers,

Due to the upgrade of Agora products, Windows SDK 2.0 will no longer support the Agora Recording Server version 1.8.2 and before, or relevant APIs.

This may affect:

-   Users of both the Windows SDK and Agora Recording Server version 1.8.2 or before.
-   The following APIs will no longer be supported, including startRecordingService,stopRecordingService, and refreshRecordingServiceStatus.


Two solutions are available:

1.  Migrate from the Agora Recording Server version 1.8.2 or before to the Agora Recording SDK version 1.12 or later. The Agora Recording SDK does not require recording on the client side and does not affect subsequent upgrades of the Windows SDK (recommended). For how to use the Agora Recording SDK, refer to:

    -   [Quickstart Guides](../../en/API%20Reference/recording_cpp-1.md)
    -   [API Reference](../../en/API%20Reference/recording_cpp.md)

2.  For users who want to continue using the Agora Recording Server, you can use your current Windows SDK version (version 1.14 and before). Contact Technical Support for any questions.

## v2.2.1

The version 2.2.0 was released on May 30, 2018. Improved the internal code implementation. 

## v2.2.0

The version 2.2.0 was released on May 4, 2018. See below for new features and improvements.

### New Features

#### 1. Play the audio effect in the channel

Added a <code>publish</code> parameter in the <code>playEffect</code> method, to enable the remote user in the channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the Proxy at the server

Agora has provided a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. For details, see descriptions in [Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy.md).

#### 3. Get the remote video state

Added the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

#### 4. Add watermarks on the broadcasting video

Added the watermark function which enables users to add a PNG file to the local or CDN broadcast as a watermark. Added <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add (and delete) watermarks in a local live-broadcast; the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface to enable watermark in the CDN broadcast. 

### Improvements

#### 1. Audio volume indication

Improved the function of <code>enableAudioVolumeIndication</code>. The method, once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether any one is speaking in the channel

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method has improved its data accuracy. 

#### 3. Lastmile quality detection before joining a channel

To test if the customers’ network condition can support audio or video calls before joining the channel, <code>onLastmileQuality</code> has changed its detection base from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, this callback is still triggered at a time interval of 2 seconds. 

#### 4. Audio Quality Enhancement

Improved the audio quality in scenarios that involve music play.

### Issues Fixed

-   Fixed occasional screen display abnormalities when a large number of audience join host in the live-broadcast channel.
-   Fixed occasional audio block issues during a live broadcast.


## v2.1.3

The version 2.1.3 was released on April 19, 2018. See below for improvements andissues fixed.

In v2.1.3 Agora has updated the bitrate values of the <code>setVideoProfile</code> method in the live-broadcast mode. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

Fixed the issue of occasional recording failure on some phones when the user leaves the channel and turns on the in-built recording device.

### Improvements

Improved the performance of screen sharing by shortening the time interval between which users switch from screen sharing to normal communication or live-broadcast.

## v2.1.2

The version 2.1.2 was released on April 2, 2018. See below for issues fixed.

>  If you upgraded the SDK to v2.1.2 from a previous version, the live-broadcast video quality will be better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth.

### Issues Fixed

-   The co-host cannot be seen by his/her counterpart after leaving and rejoining the channel.
-   The video resolution of the shared screen is worse in communication mode than in live broadcast mode.


## v2.1.1

The version 2.1.1 was released on March 16, 2018. 

Agora has identified a critical bug in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0

The version 2.1.0 was released on March 7, 2018. See below for new features, improvements, and issues fixed.

### New Features

#### 1. Voice Optimization

Added a scenario for the game chat room to reduce the bandwidth and cancel the noise with the  <code>setAudioProfile</code> method.

#### 2. Enhanced audio effect input from built-in microphone

In an Interactive Broadcast scenario, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods to implement the expected voice equalization and reverberation effects.

#### 3. Online Statistics Query

Added Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or host. See:

-   In a Voice or Video Call scenario, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md)
-   In an Interactive Broadcast scenario, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md)


#### 4. 17-way Video

Added the support of 17-way video in an Interactive Broadcast scenario, see:

-   [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_ios.md)
-   [Video Conference of 7+ Users](../../en/Voice/seventeen_people_windows.md)


#### 5. Injecting an External Video Stream

Added the function of injecting an external video stream to an ongoing Live Broadcast, see [Injecting an External Stream to a Live Broadcast](../../en/Quickstart%20Guide/inject_stream_ios.md) for details.


#### 6. Screen Sharing for Interactive Broadcast

-   Before v2.1.0: Agora SDK only supports the screen sharing function in a video call scenario;
-   From v2.1.0: Agora SDK supports the screen sharing function in an Interactive Broadcast scenario as well;


#### 7. Loopback Recording

Added an interface <code>enableLoopbackRecording</code> to collect all the local sounds.

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
<td>Reduced the video freeze rate in an audience mode and for specific devices.</td>
</tr>
<tr><td>Authentication</td>
<td>Supported a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each Token in the new authentication mechanism includes all the privileges (for example, joining a channel, host-in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions will be billed according to the voice-only mode, for example, 16 &times; 16.</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasional issues of the camera unable to capture the video source.
-   Occasional video freeze issues.
-   Occasional crashes.


## v2.0 and Earlier
### v2.0

The version 2.0 was released on November 21, 2017. See below for new features.

#### New Features

-   Added the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the communication scenario to support dual stream.
-   Supported the external audio source in the communication and live broadcast scenarios by adding the following APIs:

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
<td>Sets the external audio source parameters, including the sampling rate and channel number.</td>
</tr>
<tr><td><code>pushExternalAudioFrame</code></td>
<td>Pushes the external audio frame to the Agora SDK.</td>
</tr>
</tbody>
</table>



-   Provided a set of RESTful APIs to kick out a peer user from the server in the communication and live broadcast scenarios. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable the function if required.
-   Supported Windows (x64).
-   Added APIs such as volume, mute, and system volume settings.

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

The version 1.14 was released on October 20, 2017. See below for new features, improvements and issues.

#### New Features

-   Added the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Added the <code>setLocalVoicePitch</code> method to set the local voice pitch.

-   Live Broadcast: Added the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimized the audio at high bitrates.
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (226 ms on average, and 204 ms under good network conditions).
-   Added the ability to reduce the bandwidth:
    -   Before v1.14: If you muted the audio or disabled the video of a specific user, the network still sent the streams.
    -   Starting from v1.14: If you mute the audio or disable the video of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was in poor conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


#### Issues Fixed:

-   Unable to hear any audio on certain Windows systems.
-   Camera-related issues on certain Windows systems.


### v1.13.1

The version 1.13.1 was released on September 28, 2017. 

Optimized the echo issue under certain circumstances.

### v1.13

The version 1.13 was released on September 4, 2017. See below for new features, improvements, and issues fixed.

#### New Features:

-   Added the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Added the function to disable the audio playback.
-   Supported the profile configuration for stream-pushing on the client side.
-   Added the <code>onClientRoleChanged</code> callback function to indicate a user role change between the host and audience in a live broadcast.
-   Supported the push-stream failure callback on the server side.


#### Improvements:

-   Screen Sharing: Enhanced the video definition and fluency.
-   Screen Sharing: Supported updating the captured region dynamically.
-   The video profile is controllable by the software codec.


#### Issues Fixed:

Occasional crashes.

### v1.12

The version 1.12  was released on July 25, 2017. See below for new features, inprovements and issues fixed.

#### New Features:

-   Added the <code>injectStream</code> method to inject an RTMP stream into the current channel in a live broadcast scenario.
-   Added the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code>  method.
-   Added the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Added a set of APIs to manage the audio effect.
-   Added the <code>ActiveSpeaker</code> method to indicate who is the active speaker in the current channel.
-   Removed the <code>setScreenCaptureWindow</code> method, and updated the <code>startScreenCapture</code> method to share the whole screen and specify the window or region in the communication scenario.
-   Added displaying the mouse function when the screen sharing function is enabled in the communication scenario.
-   Web: Added and updated a series of APIs to enable interoperability between the web browsers and native clients for communication or live broadcast scenarios. For details, refer to [Release Notes](../../en/Voice/release_web_video.md).


Recording: Added real-time video mixing, web recording, and callback functions. For details, refer to [Release Notes for the Recording SDK](../../en/Product%20Overview/release_recording.md).

#### Improvements:

Android/iOS/macOS/Windows: In the communication scenario, an improvement for the 320 &times; 180 resolution profile:

- Keep the video smooth under poor network and equipment conditions.
- Enhance the image quality to be better than 180p under good network and equipment conditions.



#### Issues Fixed:

- Android: Bluetooth issues related to audio routing.
- Android/iOS/macOS/Windows: Occasional crashes.







