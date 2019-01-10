
---
title: Release Notes
description: 
platform: macOS
updatedAt: Thu Jan 10 2019 05:21:57 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Video SDK for macOS.

## Overview

The Agora Video SDK supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

### Known Issues and Limitations

A USB device driver issue occurs when users do not hear any audio or the audio is corrupted with a USB headset. USB is not user-friendly on macOS, and the use of higher quality headsets is recommended.

## v2.2.3

v2.2.3 was released on July 5, 2018. 

>  The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version earlier than v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

### Issues Fixed

-   Occasional online statistics crashes.
-   Occasional crashes during a live broadcast.
-   Excessive increase in the memory use when multiple delegated hosts broadcast in the channel.
-   Occasional video freeze after a view size change.
-   Failing to report the uid and volume of the speaker in a channel.


## v2.2.0

v2.2.0 was released on May 4, 2018. 

### New Features

#### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method for the remote user in the channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from an earlier version, pay attention to the functional changes of this method.

#### 2. Deploy the proxy at the server

Agora provides a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. 

#### 3. Get the remote video state

Adds the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

#### 4. Add watermarks on the broadcasting video

Adds the watermark function which enables users to add a PNG file to a local or CDN broadcast as a watermark. Adds the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add and delete watermarks in a local live-broadcast. The <code>watermark</code> parameter in the <code>LiveTranscording</code> interface enables watermarks in CDN broadcasts. 

### Improvements

#### 1. Audio volume indication

Improves the function of the <code>enableAudioVolumeIndication</code> method. The method, once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method has improved its data accuracy. 

#### 3. Last-mile network quality detection before joining a channel

To test if the customers’ network condition can support voice or video calls before joining the channel, the <code>onLastmileQuality</code> callback changes its detection from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, the SDK still triggers this callback once every two seconds. 

#### 4. Audio quality enhancement

Improves the audio quality in music playback scenarios.

### Issues Fixed

-   Occasional crashes on the macOS device.
-   Occasional screen display abnormalities when a large number of the audience joins as hosts in the live-broadcast channel.


## v2.1.3

v2.1.3 was released on April 19, 2018. 

In v2.1.3, Agora updates the bitrate values of the <code>setVideoProfile</code> method in the live-broadcast mode. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

-   Block callbacks are occasionally not received if the delegate is not set.
-   NSAssertionHandler appears in external links to the SDK.
-   Occasional recording failures on some phones when the user leaves the channel and turns on the built-in recording device.


### Improvements

Improves the performance of screen sharing by shortening the time interval between which users switch from screen sharing to normal communication or live-broadcast mode.

## v2.1.2

v2.1.2 was released on April 2, 2018. 

>  If you upgrade the SDK to v2.1.2 from an earlier version, the live-broadcast video quality is better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth. 

### New Features

Extends the <code>setVideoProfile</code> method for users to manually set the resolution, frame rate, and bitrate of the video. 

### Issues Fixed

The video resolution of the shared screen is worse in the communication profile than in the live broadcast profile.

## v2.1.1

v2.1.1 was released on March 16, 2018. 

Agora identified a critical issue in v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0

v2.1.0 was released on March 7, 2018. 

### New Features

#### 1. Voice Optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhanced audio effect input from a built-in microphone

In an interactive broadcast, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online statistics query

Adds Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

-   For voice or video calls, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
-   For interactive broadcasts, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).


#### 4. 17-way video

Adds the support of 17-way video in interactive broadcasts, see:

-   [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_mac.md)
-   [Video Conference of 7+ Users](../../en/Video/seventeen_people_iosmac.md)


#### 5. Video source customization

Supports the default video-capturing features provided by the camera and the customized video source. 

#### 6. Renderer customization

Supports the default functions provided by the renderers to display the local and remote videos to meet developers’ requirements. Agora provides a set of interfaces for customized renderers. 

#### 7. Screen sharing for interactive broadcast

-   Before v2.1.0: The Agora SDK supported the screen-sharing function only in video calls.
-   From v2.1.0: The Agora SDK supports the screen-sharing function in video-calls and interactive-broadcasts.


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
<td>Supports a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each token in the new authentication mechanism includes all privileges (for example, joining a channel, hosting in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions are billed according to voice-only mode. For example, 16 x 16.</td>
</tr>
<tr><td>API Naming Optimization</td>
<td>Modifies a set of names for the API attributes and enumeration values.</td>
</tr>
</tbody>
</table>



## v2.0.2

v2.0.2 was released on December 15, 2017 and fixes the FFmpeg symbol conflict.

## v2.0 and Earlier
### v2.0

v2.0 was released on December 6, 2017. 

#### New Features

-   Adds the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the Communication profile to support dual streams.
-   Updates the following callbacks for audio mixing and sound effects:

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
<tr><td><code>rtcEngineMediaEngineDidAudioMixingFinish</code></td>
<td>Removed. Replaced by rtcEngineLocalAudioMixingDidFinish.</td>
</tr>
<tr><td><code>rtcEngineDidAudioEffectFinish</code></td>
<td>Added. Notifies that the local audio effect has stopped.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidStart</code></td>
<td>Added. Notifies that the remote user has started the audio mixing.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidFinish</code></td>
<td>Added. Notifies that the remote user has stopped the audio mixing.</td>
</tr>
</tbody>
</table>


-   Adds the camera management function in the Communication and Live-broadcast profiles by adding the following API methods:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbhead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>isCameraZoomSupported</code></td>
<td>Checks whether the device supports camera zoom.</td>
</tr>
<tr><td><code>isCameraTorchSupported</code></td>
<td>Checks whether the device supports camera flash.</td>
</tr>
<tr><td><code>isCameraFocusPositionInReviewSupported</code></td>
<td>Checks whether the device supports camera manual focus.</td>
</tr>
<tr><td><code>isCameraAutoFocusFaceModeSupported</code></td>
<td>Checks whether the device supports camera auto-face focus.</td>
</tr>
<tr><td><code>setCameraZoomFactor</code></td>
<td>Sets the camera zoom ratio.</td>
</tr>
<tr><td><code>setCameraFocusPositionInPreview</code></td>
<td>Sets the position for manual focus and activates focusing.</td>
</tr>
<tr><td><code>setCameraTorchOn</code></td>
<td>Sets the device to turn on the camera flash.</td>
</tr>
<tr><td><code>setCameraAutoFocusFaceModeEnabled</code></td>
<td>Sets the device to start auto-face focusing.</td>
</tr>
</tbody>
</table>

-   Supports an external audio source in the communication and live broadcast profiles by adding the following API methods:

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
<td>Enables an external audio source.</td>
</tr>
<tr><td><code>disableExternalAudioSource</code></td>
<td>Disables the external audio source.</td>
</tr>
<tr><td><code>pushExternalAudioFrameRawData</code></td>
<td>Pushes an external audio frame to the Agora SDK.</td>
</tr>
</tbody>
</table>

-   Provides a set of RESTful APIs to ban a peer user from the server in the Communication and Live-broadcast profiles. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function, if required.


#### Issues Fixed

Audio routing and Bluetooth issues.

### v1.14

v1.14 was released on October 20, 2017. 

#### New Features

-   Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch.
-   Live Broadcast: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimizes the audio at high bitrates.
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (858 ms on average, and 625 ms when in good network conditions).
-   Adds the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was in poor conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


### v1.13.1

v1.13.1 was released on September 28, 2017, and optimized the echo issue under certain circumstances.

### v1.13

v1.13 was released on September 4, 2017.

#### New Features

-   Adds the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Adds the function to disable the audio playback.
-   Adds the module map for the SDK, which means bridging header files are not necessary for Swift projects.
-   Supports the profile configuration for stream-pushing on the client side.
-   Adds the <code>didClientRoleChanged</code> callback to indicate a user role change between a host and an audience in a live broadcast.
-   Supports the push-stream failure callback on the server side.


#### Improvements:

-   Screen Sharing: Enhances the video definition and fluency.
-   Screen Sharing: Supports updating the captured region dynamically.
-   The video profile is controllable by the software codec.


#### Issues Fixed:

Occasional crashes on some devices.

### v1.12

v1.12  was released on July 25, 2017. 

#### New Functions:

-   Adds the <code>injectStream</code> method to inject an RTMP stream into the current channel in a live broadcast scenario.
-   Adds the <code>aes-128-ecb</code> encryption mode in the `setEncryptionMode` method.
-   Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Adds a set of API methods to manage the audio effect.
-   Adds the <code>ActiveSpeaker</code> method to indicate who is the active speaker in the current channel.
-   Removes the <code>setScreenCaptureWindow</code> method, and updates the <code>startScreenCapture</code> method to share the whole screen and specify the window or region in the Communication profile.
-   Adds the displaying the mouse function when the screen-sharing function is enabled in the Communication profile.


#### Improvements:

In the Communication profile, improves the 320 &times; 180 resolution profile.

-   Keeps the video smooth under poor network and equipment conditions.
-   Enhances the image quality better than 180p under good network and equipment conditions.


#### Issues Fixed:

Occasional crashes.



