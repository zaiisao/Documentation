
---
title: Release Notes
description: 
platform: macOS
updatedAt: Wed Jan 09 2019 17:44:44 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Video SDK for macOS.

## Overview

The Video SDK supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms) and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

### Known Issues and Limitations

A USB device driver issue occurs when users do not hear any audio or the audio is corrupted with a USB headset. USB is not user-friendly on macOS, and the use of higher quality headsets is recommended.

## v2.2.3

The version 2.2.3 was released on July 5, 2018. See below for issues fixed.

>  The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version earlier than v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

### Issues Fixed

-   Fixed occasional online statistics crashes.
-   Fixed occasional crashes during a live broadcast.
-   Fixed the excessive increase in the memory use when multiple delegated hosts broadcast in the channel.
-   Fixed the issue of occasional video freeze after a view size change.
-   Fixed the issue of failing to report the uid and volume of the speaker in a channel.


## v2.2.0

The version 2.2.0 was released on May 4, 2018. See below for new features and improvements.

### New Features

#### 1. Play the audio effect in the channel

Added a <code>publish</code> parameter in the <code>playEffect</code> method, to enable the remote user in the channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the proxy at the server

Agora has provided a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. 

#### 3. Get the remote video state

Added the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

#### 4. Add watermarks on the broadcasting video

Added the watermark function which enables users to add a PNG file to the local or CDN broadcast as a watermark. Added the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> APIs to add and delete watermarks in a local live-broadcast; the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface enables watermarks in CDN broadcasts. 

### Improvements

#### 1. Audio volume indication

Improved the function of <code>enableAudioVolumeIndication</code>. The method, once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether any one is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method has improved its data accuracy. 

#### 3. Lastmile quality detection before joining a channel

To test if the customers’ network condition can support voice or video calls before joining the channel, <code>onLastmileQuality</code> changed its detection from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, this callback is still triggered at two-second intervals. 

#### 4. Audio Quality Enhancement

Improved the audio quality in scenarios that involve music play.

### Issues Fixed

-   Fixed occasional crashes on the macOS device.
-   Fixed occasional screen display abnormalities when a large number of audience join host in the live-broadcast channel.


## v2.1.3

The version 2.1.3 was released on April 19, 2018. See below for issues fixed.

In v2.1.3, Agora has updated the bitrate values of the <code>setVideoProfile</code> method in the live-broadcast mode. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

-   Block callbacks were occasionally not received if the delegate was not set.
-   NSAssertionHandler appeared in external links to the SDK.
-   Occasional recording failures on some phones when the user leaves the channel and turns on the in-built recording device.


### Improvements

Improved the performance of screen sharing by shortening the time interval between which users switch from screen sharing to normal communication or live-broadcast mode.

## v2.1.2

he version 2.1.2 was released on April 2, 2018. See below for issues fixed.

>  If you upgraded the SDK to v2.1.2 from a previous version, the live-broadcast video quality will be better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth. 

### New Features

Extended the <code>setVideoProfile</code> method to enable users to manually set the resolution, frame rate, and bitrate of the video. 

### Issues Fixed

The video resolution of the shared screen is worse in communication mode than in live broadcast mode.

## v2.1.1

The version 2.1.1 was released on March 16, 2018. 

Agora has identified a critical bug in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0

The version 2.1.0 was released on March 7, 2018. See below for new features, improvements, and issues fixed.

### New Features

#### 1. Voice Optimization

Added a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhanced audio effect input from a built-in microphone

In an interactive broadcast, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online Statistics Query

Added Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

-   For voice or video calls, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
-   For interactive broadcasts, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).


#### 4. 17-way Video

Added the support of 17-way video in interactive broadcasts, see:

-   [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_mac.md)
-   [Video Conference of 7+ Users](../../en/Video/seventeen_people_iosmac.md)


#### 5. Video Source Customization

Supported the default video-capturing features provided by the camera and the customized video source. 

#### 6. Renderer Customization

Supported the default functions provided by the renderers to display the local and remote videos to meet developers’ requirements. Agora provides a set of interfaces for customized renderers. 

#### 7. Screen Sharing for Interactive Broadcast

-   Before v2.1.0: The Agora SDK only supported the screen sharing function in video calls
-   From v2.1.0: The Agora SDK added the screen sharing function in interactive broadcasts.


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
<td>Reduced the video freeze rate in the audience mode and for specific devices.</td>
</tr>
<tr><td>Authentication</td>
<td>Supported a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each Token in the new authentication mechanism includes all the privileges (for example, joining a channel, hosting in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions will be billed according to voice-only mode, for example, 16 x 16.</td>
</tr>
<tr><td>API Naming Optimization</td>
<td>Modified a set of names for the API attributes and numeration values.</td>
</tr>
</tbody>
</table>



## v2.0.2

The version 2.0.2 was released on December 15, 2017. See below for issues fixed.

Fixed the FFmpeg symbol conflict.

## v2.0 and Earlier
### v2.0

The version 2.0 was released on December 6, 2017. See below for new features and issues fixed.

#### New Features

-   Added the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the communication scenario to support dual stream.
-   Updated the following callback functions for audio mixing and sound effects:

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
<td>Added. Notifies that the remote user has started the audio mxing.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidFinish</code></td>
<td>Added. Notifies that the remote user has stopped the audio mixing.</td>
</tr>
</tbody>
</table>


-   Added the camera management function in the communication and live broadcast scenarios by adding the following API methods:

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



-   Supported the external audio source in the communication and live broadcast scenarios by adding the following API methods:

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
<td>Enables the external audio source.</td>
</tr>
<tr><td><code>disableExternalAudioSource</code></td>
<td>Disables the external audio source.</td>
</tr>
<tr><td><code>pushExternalAudioFrameRawData</code></td>
<td>Pushes the external audio frame to the Agora SDK.</td>
</tr>
</tbody>
</table>



-   Provided a set of RESTful APIs to kick out a peer user from the server in the communication and live broadcast scenarios. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function if required.


#### Issues Fixed

Audio routing and Bluetooth issues.

### v1.14

The version 1.14 was released on October 20, 2017. See below for new features and improvements.

#### New Features

-   Added the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Added the <code>setLocalVoicePitch</code> method to set the local voice pitch.
-   Live Broadcast: Added the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimized the audio at high bitrates.
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (858 ms on average, and 625 ms when in good network conditions).
-   Added the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was in poor conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


### v1.13.1

The version 1.13.1 was realeased on September 28, 2017. 

Optimized the echo issue under certain circumstances.

### v1.13

The version 1.13 was released on September 4, 2017. See below for new features, improvements, and issues fixed.

#### New Features

-   Added the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Added the function to disable the audio playback.
-   Added the module map for the SDK, which means bridging header files are not necessary for Swift projects.
-   Supported the profile configuration for stream-pushing on the client side.
-   Added the <code>didClientRoleChanged</code> callback function to indicate a user role change between the host and audience in a live broadcast.
-   Supported the push-stream failure callback on the server side.


#### Improvements:

-   Screen Sharing: Enhanced the video definition and fluency.
-   Screen Sharing: Supported updating the captured region dynamically.
-   The video profile is controllable by the software codec.


#### Issues Fixed:

Occasional crashes on some devices.

### v1.12

The version 1.12  was released on July 25, 2017. See below for new features and issues fixed.

#### New Functions:

-   Added the <code>injectStream</code> method to inject an RTMP stream into the current channel in a live broadcast scenario.
-   Added the <code>aes-128-ecb</code> encryption mode in the API method, `setEncryptionMode`.
-   Added the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Added a set of API methods to manage the audio effect.
-   Added the <code>ActiveSpeaker</code> method to indicate who is the active speaker in the current channel.
-   Removed the <code>setScreenCaptureWindow</code> method, and updated the <code>startScreenCapture</code> method to share the whole screen and specify the window or region in the communication scenario.
-   Added the displaying the mouse function when the screen sharing function is enabled in the communication scenario.


#### Improvements:

In the communication scenario, improved the 320 &times; 180 resolution profile.

-   Keep the video smooth under poor network and equipment conditions.
-   Enhance the image quality better than 180p under good network and equipment conditions.


#### Issues Fixed:

Occasional crashes.



