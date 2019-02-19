
---
title: Release Notes
description: 
platform: Web
updatedAt: Tue Feb 19 2019 05:47:55 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Web SDK.

## Overview

The Agora Web SDK (WebRTC) is a JavaScript library loaded by an HTML web page. The Agora Web SDK library uses APIs in the web browser to establish connections and control the communication and live broadcast services. For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms) and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

### Compatibility

See the table below for the browser support of Agora Web SDK:

<table>
  <tr>
    <th>Platform</th>
    <th>Google Chrome 58 or later</th>
    <th>Firefox 56 or later</th>
    <th>Safari 11 or later</th>
    <th>Opera 45 or later</th>
    <th>QQ Browser</th>
    <th>360 Secure Browser</th>
    <th>WeChat Built-in Browser</th>
  </tr>
   <tr>
    <td>Android 4.1 or later</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>iOS 11 or later</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>macOS 10 or later</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>Windows 7 or later</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
  </tr>
</table>

> The Agora Web SDK v2.5 or later also supports Google Chrome 49 on Windows XP.

> To enable interoperability between the Agora Native SDK and Agora Web SDK, use the Agora Native SDK v1.12 or later.

### Known Issues and Limitations

- Affected by browser policy changes, `Stream.play`, `Stream.startAudioMixing`, and `Stream.getAudioLevel` must be triggered by the user's gesture on the Chrome 70+ and Safari browsers, see [Autoplay Policy Changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes) for details.

- The Agora Web SDK supports video profiles up to 1080p resolutions if the client has a true HD camera installed. However, the maximum resolution is limited by the camera device capabilities.
- The Agora Web SDK does not support code obfuscation.

For more issues, see [Web FAQs](../../en/Voice/websdk_related_faq.md).


## v2.5.0

The version 2.5.0 was released on October 30, 2018. See below for new features and fixed issues. 

> <font color="red">If you have set the domain firewall, ensure `*.agoraio.cn` and `*.agora.io` are added to your white list before using this version.</font>

### New Features

To enable better interoperability between Agora Web SDK and other Agora SDKs, this version has added the following features. For detailed descriptions of the APIs, see [Agora Web SDK API Reference](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/index.html).

#### 1. Quality Transparency

To give users a better idea of the call quality, the following APIs on call statistics are added:

- `Client.getNetworkStats` : Retrieves the network statistics (network type).
- `Client.getSystemStats` : Retrieves the system statistics (system battery level).
- `Client.getRemoteAudioStats` : Retrieves the audio statistics of the remote stream.
- `Client.getLocalAudioStats` : Retrieves the audio statistics of the local stream.
- `Client.getRemoteVideoStats` :  Retrieves the video statistics of the remote stream.
- `Client.getLocalVideoStats` : Retrieves the video statistics of the local stream.
- `Client.getTransportStats` : Retrieves statistics of the network transportation.

#### 2. Device Management

Provides flexible device management and device status notification.

- **Enumera Devices**

  Added the following APIs:

  - `Client.getRecordingDevices` : Enumerates the audio input device, such as the microphones.
  - `Client.getPlayoutDevices` : Enumerates the audio output device, such as the speakers.
  - `Client.getCameras` : Enumerates the video input device, such as the cameras.

  Added the following callbacks to inform the application that the relevant device is changed:

  - `recordingDeviceChanged` : The audio input device is changed.
  - `playoutDeviceChanged` : The audio output device is changed.
  - `cameraChanged` : The video inout device is changed.

- **Switch Devices**

  Added the following APIs:

  - `Steream.switchDevice` :  Switches the media input devices in the channel, for example the microphone and camera.
  - `Stream.setAudioOutput` : Sets the audio output device. You can use this API to switch between the microphone and the speaker.

#### 3. Audio Mixing

Supports audio mixing, namely mixing the original sound (audio captured by the microphone) with the playback (an audio file).

 The following APIs are added:

- `Stream.startAudioMixing` : Starts audio mixing.
- `Stream.stopAudioMixing` : Stops audio mixing.
- `Stream.pauseAudioMixing` : Pauses audio mixing.
- `Stream.resumeAudioMixing` : Resumes audio mixing.
- `Stream.adjustAudioMixingVolume` : Adjusts the audio mixing volume.
- `Stream.getAudioMixingDuration` : Retrieves the audio mixing duration.
- `Stream.getAudioMixingCurrentPosition` : Retrieves the audio mixing current position.
- `Stream.setAudioMixingPosition` : Sets the playing position of the audio mixing.

#### 4. Audio/Video Track Management

Provides flexible management of the audio and video tracks.

Added the following APIs:

- `Stream.getAudioTrack` : Retrieves the audio track.
- `Stream.getVideoTrack` : Retrieves the video track.
- `Stream.replaceTrack` :  Replaces the audio/video track.
- `Stream.addTrack` : Adds an audio/video track.
- `Stream.removeTrack` : Removes an audio/video track.

#### 5. Other New Features

- Support for two video display modes. You can set the display mode in  `Stream.play`.
- Added the `Client.enableAudioVolumeIndicator` method to enable the SDK regularly reports the active speakers and their volumes.
- Added the `Stream.setAudioVolume` method, which sets the audio volume of the subscribed stream.
- Added the `networkTypeChanged` callback to inform the application that the network type is changed.
- Adde the `streamTypeChanged` callback to infrom the application that the stream type has changed (from low-video stream to high-video stream or vice versa).
- Support for both string and number types for the `uid` parameter in  `Client.join`.
- Support for 360 Security Browser 9.1.0.432+.
- Support for Chrome 49 on the Windows XP system.

### Issues Fixed

- Fixed the dependency on the video codec in audio-only calls when the user joins the channel from Safari or Chrome on mobile devices.
- Fixed the failure to receive the `stream-removed` callback 10 seconds after the other user has called `Stream.close` to stop streaming from the Safari browser.
- Fixed the issue of receiving warning after the user has used the same UID to reset `Stream.userID`.


## v2.4.1

The version 2.4.1 was released on September 19, 2018. See below for issues fixed.

### Issues Fixed

- The local publisher would not automatically publish after IP address change, so the remote subscriber could not receive the video stream.
- Unsubscribing from the remote stream triggered the `stream-removed` event.

## v2.4.0

The version 2.4.0 was released on August 24, 2018. See below for new features and issues fixed.

### New Features

#### 1. Support for Token

See [Security Keys](../../en/Agora%20Platform/token.md) for details.

#### 2. Support for Audio Processing

Added support for AEC (Acoustic Echo Cancellation) and ANS (Automatic Noise Suppression) in the audioProcessing property. 

#### 3. Support for Audio/Video Pre-processing

Added the <code>audioSource</code> and <code>videoSource</code> properties in the <code>client.createStream</code> method, which specify the audio and video tracks. Therefore, you can process the audio/video before creating a stream. 

#### 4. Support for Setting Audio Profile

Added the <code>stream.setAudioProfile</code> method, which provides various audio profile options (sample rate, mono or stereo, and encoding rate).  

#### 5. Support for Setting Stream Fallback

Added the <code>client.setStreamFallbackOption</code> method, which allows the receiver to automatically subscribe to the low-video stream or only the audio stream under poor network conditions.

#### 6. Added Delay-related Statistics

The callback function of <code>stream.getStats</code> added delay-related statistics, including the delays from the publisher to SD-RTN, from SD-RTN to the subscriber, from end to end, and from sending to playing audio/video. 

#### 7. Support for Log Upload

Added the <code>AgoraRTC.Logger.enableLogUplaod</code> method, which supports uploading the SDK’s log to Agora’s server. 

### Issues Fixed

- The settings of <code>cameraId</code> and <code>microphoneId</code> would not take effect unless <code>stream.setVideoProfile</code> is called.
- Calling <code>setScreenProfile</code> would not take effect on the Firefox browser.
- An error occurs if both audio and screen are enabled in the <code>client.createStream</code> method.
- The <code>microphoneId</code> setting would not take effect in screen sharing.
- The <code>audioProcessing</code> settings would not take effect.
- The `stream.play` method could be called repeatedly. If you call `stream.play`, you should not be able to call it again before calling `stream.stop`.

## v2.3.1

The version 2.3.1 was released on June 7, 2018. See below for issues fixed.

### Issues Fixed

- Fixed the issue of occasional publishing failures on browsers that installed the Adblock Plus extension.
- Fixed the issue of invalid IP jumps.

## v2.3.0

The version 2.3.0 was released on June 4, 2018. See below for new features and issues fixed.

### New Features

#### 1. New Session Mode

To increase the applicable scenarios and improve its interoperability with the Agora Native SDK, the <code>mode</code> and <code>codec</code> parameters are added in the <code>createClient</code> method, in which <code>mode</code> includes <code>rtc</code> and <code>live</code>, while <code>codec</code> includes <code>vp8</code> and <code>h264</code>. 

#### 2. Support for Audio Gain Control

To meet the customers’ needs for audio control during a communication or live broadcast session, the <code>audioProcessing</code> parameter is added in the <code>createStream</code> method.

#### 3. Support for Proxy on the Web Side

To enable enterprises with a company firewall to access the Agora’s services, <code>setProxyServer</code> and <code>setTurnServer</code> methods are added. By calling these two methods, users can bypass the firewall and send the signaling messages and media data directly to the Agora SD-RTN through the servers. Users need to deploy the Nginx and TURN Server before using this function and calling the methods before joining the channel. For more information on how the proxy works, deploy steps and APIs, see [Deploying the Enterprise Proxy](../../en/Voice/proxy_web.md).

#### 4. Support for Encryption

Encryption was supported to enhance security for communication or live broadcast. Users need to set the encryption mode and password before joining the channel to use this function. 

### Issues Fixed

In the case of p2plost, the SDK stops reconnecting to the server after one or several reconnections.

## v2.2

The version 2.2 was released on April 16, 2018. See below for new features and issues fixed.

### New Features

#### 1. Get the version information.

Supported getting the version of the current Web SDK. 

#### 2. Set the low-stream parameter.

Added `setLowStreamParameter` interface to set the low-stream parameter. 

#### 3. Support screen-sharing for the Firefox browser.

Added the support for the Firefox browser to share the screen, by adding a <code>mediaSource</code> property in the <code>createStream</code> method. See [Screen Sharing on Firefox](../../en/Voice/screensharing_web.md) for details.

#### 4. Support the QQ browser.

Added the support for the QQ browser.

### Issues Fixed

Fixed the issue of no voice in the voice-only mode when an iOS device joins a session using the Safari browser.

## v2.1.1

The version 2.1.1 was released on March 19, 2018. See below for issues fixed.

Fixed the issue of not able to view the remote video when using the Web SDK on the Firefox browser v59.01 on macOS.

## v2.1.0

The version 2.1.0 was released on March 7, 2018. See below for new features, improvements, and issues fixed.

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
<tr><td>Quality Detection</td>
<td>Added callbacks to return the quality of the compatibility, microphone, camera, and network status.</td>
</tr>
<tr><td>Simulcast</td>
<td>Added the function of controlling which stream type to send or receive; in high stream or low stream.</td>
</tr>
<tr><td>User-ban Notification</td>
<td>Added an interface to notify a user of being banned from the channel.</td>
</tr>
<tr><td>RTMP Stream-Pushing</td>
<td>Added an interface to support RTMP stream-pushing.</td>
</tr>
<tr><td>Log Output Level Settings</td>
<td>Added an interface to set the log output level.</td>
</tr>
<tr><td>Mute/Unmute</td>
<td>Added an interface to mute or unmute the users in a call or an interactive broadcast.</td>
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
<tr><td>P2P Connection Establishment</td>
<td>Shortened the time from 1.8 s to 500 ms.</td>
</tr>
<tr><td>Packet Loss</td>
<td>Optimized the FEC packet loss and ULP FEC packet loss.</td>
</tr>
</tbody>
</table>



### Issues Fixed

- Fixed the 90-degree video rotation on Safari
- Fixed the issue of not seeing the remote video when using the Web SDK on the Firefox browser v59.01 on macOS

## v2.0 and Earlier
### v2.0

The version 2.0 was released on November 21, 2017. See below for new features and issues fixed.

#### New Features

- Added the check browser compatibility function before calling <code>CreateClient</code>, to check the compatibility between the system and the browser.
- Added the callback to notify the application that the user has granted or denied access to the camera or microphone.
- Added the video self-adjustment function, by automatically lowering the resolution or frame rate until the video profile matches the input hardware’s capability.

#### Issues Fixed

- Volume issue with iOS SDK interop.
- Disconnection issue on Google Chrome caused by prolonged communication.
- Abnormal screen display on Android devices when the device switches between its front and rear cameras.
- Abnormal screen display on Android devices when one of the Android devices in the session leaves the channel.
- No camera-access-grant notification on Google Chrome when it joins the channel without access to the camera.
- No image on the iOS Safari web browser after the user has returned from another application.

### v1.14

The version 1.14 was released on October 20, 2017. See below for new features.

Added the screen sharing function by modifying the screen parameter and adding the parameter in the <code>createStream</code> API.

### v1.13

The version 1.13 was released on September 4, 2017. See below for new features.

#### New Features

- Supported CDN Live with the <code>configPublisher</code> API.
- Supported Google Chrome for Android.

### v1.12 

The version 1.12 was released on July 25, 2017. See below for new features.

- Added and Updated APIs:

  <table>
  <colgroup>
  <col/>
  <col/>
  <col/>
  </colgroup>
  <thead>
  <tr><th>API</strong></th>
  <th>Type</strong></th>
  <th>Description</strong></th>
  </tr>
  </thead>
  <tbody>
  <tr><td><code>createClient</code></td>
  <td>New</td>
  <td>Creates a client object for web-only or web interop depending on which mode you set</td>
  </tr>
  <tr><td><code>renewChannelKey</code></td>
  <td>New</td>
  <td>Updates a Channel Key when the previous Channel Key is expired</td>
  </tr>
  <tr><td><code>active-speaker</code></td>
  <td>New</td>
  <td>Indicates who is the active speaker in the current channel</td>
  </tr>
  <tr><td><code>setRemoteVideoStreamType</code></td>
  <td>New</td>
  <td>Specifies the video stream type of the remote user to be received by the local when the remote user sends dual streams</td>
  </tr>
  <tr><td><code>setVideoProfile</code></td>
  <td>Updated</td>
  <td>Sets the video profile, and the default value is 480p_1</td>
  </tr>
  <tr><td><code>init</code></td>
  <td>Updated</td>
  <td>Initializes the client object</td>
  </tr>
  <tr><td><code>join</code></td>
  <td>Updated</td>
  <td>Allows the user to join an Agora channel</td>
  </tr>
  </tbody>
  </table>

- Updated the error codes.

### v1.8.1

The version 1.8.1 was released on March 16, 2017. See below for issues fixed.

Fixed the incompatibility issue on Google Chrome v57.

### v1.8

The version 1.8 was released on December 26, 2016. 

The Agora Web SDK supports both communication and live broadcast scenarios starting from v1.8. This release is a beta version.
