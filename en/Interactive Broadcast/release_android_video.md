
---
title: Release Notes
description: 
platform: Android
updatedAt: Fri Jan 04 2019 08:00:52 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Video SDK for Android.

## Overview

The Video SDK supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms) and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

## v2.3.1

The version 2.3.1 was released on Oct. 12th, 2018. See below for new features, improvements, and issues fixed.

### New features

#### Disables/Re-enables the Local Audio Function

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio stream after joining a channel, this release adds the `enableLocalAudio` method is to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK returns the `onMicrophoneEnabled` callback function, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.

The difference between this method and `muteLocalAudioStream` is that `enableLocalAudio`  does not capture or send any audio stream, while `muteLocalAudioStream` captures but does not send audio streams.


### Improvements

- Improved the SDK's adaptiveness to the Android video encoder.

### Issues Fixed

- In the live broadcast profile: Occasional crashes on some Android devices after the user repeats the process of switching roles between the BROADCASTER and AUDIENCE.
- Occasional crashes when switching between front and rear cameras.
- In the communication profile: If a user disables the video and re-enables it during a one-to-one call, it takes a long time for the other user to see the image.
- In the live broadcast profile: Delay at the client due to incorrect statistics.
- Occasional failures to render the video on some Android devices.
- Occasional image freezes on some Android devices.
- Occasionally on some Android devices: A user hears a popping sound if the user leaves the channel at the same time another user is speaking.
- The timestamp in the captured raw video frames does not refresh with the frame.

## v2.3.0

The version 2.3.0 was released on August 31, 2018. See below for new features, improvements, issues fixed and API Changes.

### Before Reading

-   To support video rotation and enable better quality for the custom video source, this release deprecates the `setVideoProfile` method and uses `setVideoEncoderConfiguration` instead to set the video encoding configurations. You can still use `setVideoProfile`, but Agora recommends using `setVideoEncoderConfiguration` to set the video profile because:
    -   During a live broadcast, users can set the video orientation mode as adaptive, under which the SDK can transfer rotated video frames without cropping them, thus avoiding the “big headshot” or blurry images at the player.
    -   In scenarios involving external video sources, the SDK adjusts the width and height of the output video frames based on the inputting video frames, avoiding unnecessary cropping and thereby rendering more image frames at the player.
-   From v2.3.0, the LiveTranscoding Class was moved from the *io.agora.live* package to the `io.agora.rtc.live` package.
-   Fixed a typo in the constants.java API in v2.3.0.
    -   Before:

    ```
    public static final int SOFEWARE_ENCODER = 1;
    ```

    -   After:

    ```
    public static final int SOFTWARE_ENCODER = 1;
    ```

-   The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).


### New Features

#### 1. Fallback options for a live broadcast under unreliable network conditions

The audio and video quality of a live broadcast will deteriorate under unreliable network conditions. To improve the efficiency of a live broadcast, the `setLocalPublishFallbackOption` and `setRemoteSubscribeFallbackOption` interfaces are added. These interfaces allow the SDK to automatically disable the video stream when the network conditions cannot support both audio and video, and enable the video when the network conditions improve. `onLocalPublishFallbackToAudioOnly` or `onRemoteSubscribeFallbackToAudioOnly` is triggered when the stream falls back to audio-only or when the stream switches back to the video.

#### 2. Notifies the user that the Token will expire in 30 seconds

The SDK returns the `onTokenPrivilegeWillExpire` callback function 30 seconds before a Token expires to notify the app to renew it. When this callback function is received, you need to generate a new Token on your server and call `renewToken` to pass the newly-generated Token to the SDK.

#### 3. Returns user-specific upstream and downstream statistics, including the bitrate, frame rate, packet loss rate, and time delay

The `onRemoteAudioTransportStats` and `onRemoteVideoTransportStats` callback functions are added to provide user-specific upstream and downstream statistics, including the bitrate, frame rate, and packet loss rate. During a call or a live broadcast, these callback functions are triggered once every two seconds after the user receives audio/video packets from a remote user. The callbacks include the user ID, audio bitrate at the receiver, packet loss rate, and time delay (ms).

#### 4. Sets the video encoder configurations

To support scenarios with video rotation and enable better quality for the custom video source, this release deprecates the `setVideoProfile` method and uses `setVideoEncoderConfiguration` instead to set the video encoding configurations. You can still use `setVideoProfile`, but Agora recommends using `setVideoEncoderConfiguration` to set the video profile because:

-   During a live broadcast, users can set the video orientation mode as adaptive, under which the SDK can transfer rotated video frames without cropping them, thus avoiding the “big headshot” or blurry images at the player.

-   In scenarios involving external video sources, the SDK adjusts the width and height of the output video frames  based on the inputting video frames, avoiding unnecessary cropping and thereby rendering more image frames at the player.


The <code>VideoEncoderConfiguration</code> class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. For more information on the API, see `Set the Video Encoder Configuration`.

#### 6. Adds support for background image settings in setLiveTranscoding

The backgroundImage parameter is added to the `setLiveTranscoding` method allowing you to set the background image in the combined video of a live broadcast.

### Improvements

-   Improved the quality for one-on-one voice/video scenarios with optimized latency and smoothness, especially for areas like Southeast Asia, South America, Africa, and the Middle East.
-   Improved the audio encoder efficiency in a live broadcast to reduce user traffic while ensuring the call quality.
-   Improved the audio quality during a call or a live broadcast using the deep-learning algorithm.


### Issues Fixed


- Increased memory usage when multiple delegated hosts broadcast in the channel.
- Occasional crashes on some Android devices.
- The remote view does not display on some devices.
- The local video cannot be enabled on some Android devices.
- Occasional ghost images.
- Occasional green lines at the bottom of the video when a user switches from a low stream to a high stream in the communication mode.
- Occasional crashes after interoperating with devices of other platforms for some Android devices.
- Excessive increase in the memory usage for the host when the host frequently joins and leaves a channel that has multiple delegated hosts.
- Occasional black screens on some Android devices.
- Occasionally, the remote user cannot hear the host when the host switches between the AUDIENCE and  BROADCASTER.
- Occasionally, the settings applied to the background image of a live transcoding do not take effect.
- Occasionally on some devices, the video height and width are swapped in the communication mode.
- Occasionally, the destroy method does not respond after a user enables the video and joins a channel.
- Occasional crashes on Android devices when remote users frequently join and leave the channel.
- Black screen due to failure to render the remote video on some Android devices.
- Occasionally, the audience cannot adjust the channel volume.
- Occasionally, applications do not respond on some Android devices.
- Occasional crashes on some Android devices when switching video resolutions in a live broadcast.
- A delegated host cannot see the video of the other hosts in the channel on some Android devices.
- The bitrates cannot reach the target values on some Android devices when a user frequently joins and leaves the communication channel with different video profiles.
- Occasional failures to capture the video of the delegated host when the hosts and the audience frequently change roles.
- Occasional failures to capture videos or publish streams on some Android devices when a user frequently joins and leaves a communication channel with different video profiles.
- Occasional crashes when calling the <code>setCameraFocusPositionInPreview</code> method on some devices.
- Occasional failures to enable the camera during communication on some Android devices.
- Occasional video freezes and stream publishing hangs on some Android devices after a user joins a communication or live broadcast channel.
- Occasional crashes when one of the two broadcasters mutes or disables the local audio while playing the background music.
- A user cannot join a communication channel after frequently changing the video encoder profiles.
- Occasional crashes on some devices when preloading the sound effects.
- Failure to render videos of lower resolutions on some Android devices.
- Occasionally, an Android client can still interoperate in a communication channel when removed from the dashboard.
- Video resolution inconsistencies between the encoder and the decoder in the live broadcast mode.
- Failure to enable the hardware encoder on some Android devices.
- Occasional video freezes in the communication or live broadcast mode.
- Occasional crashes when calling the <code>muteRemoteVideoStream</code> method after joining the channel.
- Occasional video freezes on some Android devices when switching from the communication mode to the live broadcast mode.
- Occasional crashes on some Android devices when frequently turning on and off the flashlight during a live broadcast.
- The host cannot receive the audio/video stream of the delegated host on some Android devices.
- Occasional crashes on some Android devices when setting the video encoder profile of an external video source during a live broadcast.
- Incorrect video orientation on some Android devices when setting the video profile of an external video source during a live broadcast.
- Occasionally on some Android devices, the video fallback option does not take effect under poor network conditions.
- Occasional crashes on some Android devices when a user frequently changes the Token.
- Occasional failures to split the screen on some Android devices.
- The CDN audience on some Android devices cannot see the video of the host.
- Occasional video freezes when switching from multiple hosts to a single host.
- Occasional inter-operational failures between SIP devices and the SDK.
- Occasionally on Mi 8, the local video cannot be seen locally or remotely.
- Occasionally on some Android devices, users cannot see each other.
- Occasional echoes when using a specific audio card.
- Occasional video delays on some Android devices.
- Occasional crashes on some Android devices when transmitting the video streams.


### API Changes

To improve the user experience, Agora has made the following changes to the APIs:

To avoid adding too many users with the same uid into the CDN publishing channel, the following API method is deleted in v2.3.0, and the return value type of `addUser` is changed from void to int.

-   <code>setUser</code>


The following API methods are deleted and no longer supported in v2.3.0. Agora provides the Recording SDK for better recording services. For more information on the Recording SDK, see [Release Notes for Agora Recording SDK](../../en/Product%20Overview/release_recording.md).

-   <code>startRecordingService</code>
-   <code>stopRecordingService</code>
-   <code>refreshRecordingServiceStatus</code>


The following deprecated API methods are deleted and no longer supported from v2.3.0:

-   <code>monitorConnectionEvent</code>
-   <code>monitorBluetoothHeadsetEvent</code>
-   <code>monitorHeadsetEvent</code>
-   <code>setPreferHeadset</code>
-   <code>switchView</code>
-   <code>setSpeakerphoneVolume</code>


### Backwards Compatibility Issues

None.

### Known Issues

None.

## v2.2.3 

The version 2.2.3 was released on July 5, 2018. See below for issues fixed.

### Read This First

The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see `Token Migration Guide`.

### Issues Fixed

- Occasional online statistics crashes.
- The broadcaster’s voice distorts occasionally on some Android devices.
- Occasional crashes during a live broadcast.
- Excessive increase in the memory usage when multiple delegated hosts broadcast in the channel.
- Receiving the <code>onLeaveChannel</code> callback long after a user has left the channel on some Android devices.
- Failing to report the uid and volume of the speaker in a channel.
- Unsteady voice volume of the broadcaster in a live broadcast.
- Occasional video freezes during a live broadcast.
- Occasional ANR (application no response) problem on some Android devices after the user turns off the camera to end a video session.
- Occasional video freeze after a view size change.



## v2.2.1

The version 2.2.1 was released on May 30, 2018. See below for issues fixed.

### Issues Fixed

- Occasional crashes during gaming on some Android devices.
- The soundtrack pointer cannot be retrieved on some Android devices.
- Occasional crashes on some Android devices.
- The audio volume on some Android devices cannot be adjusted after a headset is plugged in.


## v2.2.0

The version 2.2.0 was released on May 4, 2018. See below for new features, improvements and issues fixed.

### New Features

#### 1. Play the audio effect in the channel

Added a <code>publish</code> parameter in the <code>playEffect</code> method, to enable the remote user in the channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the proxy at the server

Agora has provided a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. For details, see [Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy.md).

#### 3. Get the remote video state

Added the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 
#### 4. Add watermarks on the broadcasting video

Added the watermark function which enables users to add a PNG file to the local or CDN broadcast as a watermark. Added the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add and delete watermarks in a local live-broadcast; the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface enables watermarks in CDN broadcasts.

### Improvements

#### 1. Audio volume indication

Improved the function of <code>enableAudioVolumeIndication</code>. This method once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method has improved its data accuracy. 

#### 3. Last mile network quality detection before joining a channel

To test if the customers’ network condition can support voice or video calls before joining the channel, <code>onLastmileQuality</code> changed the detection from a fixed bitrate to the bitrate set by the customer in <code>setVideoProfile</code> to improve data accuracy. When the network condition is unknown, this callback function is still triggered at two-second intervals. 

#### 4. Audio quality enhancement

Improved the audio quality in scenarios that involve music playback.

### Issues Fixed

- Occasional screen display abnormalities when a large number of the audience join as the host in the live-broadcast channel.
- Fixed occasional CDN streaming abnormalities when some app switches to run in the background during a live-broadcast.


## v2.1.3

The version 2.1.3 was released on April 19, 2018. See below for improvements and issues fixed.

In v2.1.3, Agora has updated the bitrate values of the <code>setVideoProfile</code> method in the live-broadcast mode. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

Occasional recording failures on some phones when the user leaves the channel and turns on the built-in recording device.

### Improvements

Improved the performance of screen sharing by shortening the time interval between which users switch from screen sharing to a normal communication or live-broadcast mode.

## v2.1.2

The version 2.1.2 was released on April 2, 2018. See below for new features and issues fixed.

>  If you upgraded the SDK to v2.1.2 from a previous version, the live-broadcast video quality will be better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth.

### New Features

Extended the <code>setVideoProfile</code> method to enable users to manually set the resolution, frame rate, and bitrate of the video. 

### Issues Fixed

Video freeze in DTX + AAC mode.

## v2.1.1 

The version 2.1.1 was released on March 16, 2018. 

Agora has identified a critical issue in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0 

The version 2.1.0 was released on March 7, 2018. See below for new features, improvements, and issues fixed.

### New Features

#### 1. Voice Optimization

Added a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhanced audio effect input from a built-in microphone

In an interactive broadcast scenario, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online statistics query

Added Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

-   For voice or video calls, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
-   For interactive broadcasts, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).


#### 4. 17-way video

Added the support of 17-way video in interactive broadcasts, see:

-   [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_android.md)
-   [Video Conference of 7+ Users](../../en/Interactive%20Broadcast/seventeen_people_android.md)


#### 5. Video source customization

Supported the default video-capturing features provided by the camera and the customized video source. 

#### 6. Renderer customization

Supported the default functions provided by the renderers to display the local and remote videos to meet developers’ requirements. Agora provides a set of interfaces for customized renderers. 

#### 7. Injecting an external video stream

Added the function of injecting an external video stream to an ongoing live broadcast.

#### 8. Camera focus change

Added an <code>onCameraFocusAreaChanged</code> callback function to indicate that the camera focus area has changed.

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
<tr><td>Hardware Encoder</td>
<td>Enabled the H.264 hardware encoder on the Qualcomm, MTK, HiSilicon, and Orion chips.</td>
</tr>
<tr><td>Hardware Encoder</td>
<td>Improved the bitrate control for the hardware encoder.</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions will be billed according to the voice-only mode, for example, 16 &times; 16.</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasional playback noise on specific devices.
-   Occasional crackling voice playback on specific devices.
-   Occasional crashes.


## v2.0.2

The version 2.0.2 was released on December 15, 2017. 

Occasional audio routing issues.

## v2.0 and Earlier

### v2.0

The version 2.0 was released on December 6, 2017. See below for new features, improvements and issues fixed.

#### New Features

-   Added the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the communication scenario to support dual streams.
-   Added the camera management function in the communication and live broadcast scenarios by adding the following API methods:

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
<tr><td><code>isCameraZoomSupported</code></td>
<td>Checks whether the device supports camera zoom.</td>
</tr>
<tr><td><code>isCameraTorchSupported</code></td>
<td>Checks whether the device supports camera flash.</td>
</tr>
<tr><td><code>isCameraFocusSupported</code></td>
<td>Checks whether the device supports camera manual focus.</td>
</tr>
<tr><td><code>isCameraAutoFocusFaceModeSupported</code></td>
<td>Checks whether the device supports camera auto-face focus.</td>
</tr>
<tr><td><code>setCameraZoomFactor</code></td>
<td>Sets the camera zoom ratio.</td>
</tr>
<tr><td><code>getCameraMaxZoomFactor</code></td>
<td>Gets the maximum zoom ratio of the camera.</td>
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



-   Supported external audio sources in the communication and live broadcast scenarios by adding the following API methods:

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
<td>Enables the external audio source function</td>
</tr>
<tr><td><code>pushExternalAudioFrame</code></td>
<td>Pushes the external audio frame to the Agora SDK</td>
</tr>
</tbody>
</table>



-   Provided a set of RESTful APIs to ban a peer user from the server in the communication and live broadcast scenarios. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function if required.
-   Supported the following Android emulators: NOX, Lightning, and Xiaoyao.


#### Improvements

Optimized the hardware encoder by supporting encoding resolutions as low as 64 x 64.

#### Issues Fixed

-   Audio routing and Bluetooth issues.
-   Optimized the volume balance control.


### v1.14

The version 1.14 was released on October 20, 2017. See below for new features, improvements and issues fixed.

#### New Features

-   Added the <code>setAudioProfile</code> method to set the audio parameters and scenarios
-   Added the <code>setLocalVoicePitch</code> method to set the local voice pitch
-   Live Broadcast: Added the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor


#### Improvements

-   Optimized the audio at high bitrates
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (226 ms on average, and 204 ms under good network conditions)
-   Added the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially under poor network conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


#### Issues Fixed

Camera related issues on Android devices.

### v1.13.1

The version 1.13.1 was released on September 28, 2017. 

Optimized the echo issue under certain circumstances.

### v1.13

The version 1.13 was released on September 4, 2017. See below for new features, improvements, and issues fixed.

#### New Features

-   Added the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Added the function to disable the audio playback.
-   Supported the profile configuration for stream-pushing on the client side.
-   Added the <code>onClientRoleChanged</code> callback function to indicate a user role change between the host and audience in a live broadcast.
-   Supported the push-stream failure callback on the server side.


#### Improvements:

The video profile is controllable by the software codec.

#### Issues Fixed:

Occasional crashes on some devices

### v1.12

The version 1.12  was released on July 25, 2017. See below for new features and issues fixed.

#### New Features:

-   Added the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code> method.
-   Added the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.


#### Issues Fixed:

-   Android: Bluetooth issues related to audio routing.
-   Android/iOS/Mac/Windows: Occasional crashes.





