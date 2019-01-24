
---
title: Release Notes
description: 
platform: Windows
updatedAt: Thu Jan 24 2019 14:33:51 GMT+0000 (UTC)
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


## v2.3.3

The version 2.3.3 was released on Jan. 23rd, 2019. See below for improvements and issues fixed.

### Improvements

v2.3.3 optimizes the screen-sharing algorithm for different scenarios. The video smoothness and quality are enhanced when a user presents slides or browses websites. v2.3.3 also improves the initial image quality in the Communication profile.

### Issues Fixed

Occasional inaccurate statistics returned in the `onNetworkQuality` callback.

## v2.3.2

The version 2.3.2 was released on Jan. 16th, 2019. See below for new features, improvements, and issues fixed.

### Before Getting Started

Besides the new features and improvements mentioned below, it is worth noting that v2.3.2:

- Improves the SDK's ability to counter packet loss under unreliable network conditions.
- Improves the communication smoothness.
- Reduces video freezes in the Live Broadcast profile.
 
Before upgrading your SDK, ensure that the version is:

- Native SDK v1.11 or later.
- Web SDK v2.1 or later.

If you using an Agora SDK version v2.0.8 and wish to migrate the v2.3.2, refer to the [Migration Guide](../../en/Video/migration_windows.md) for major API changes.

### New Features

#### 1. External video data (Push Mode)

v2.3.2 adds the following methods, allowing you to use the external video during a call or in a live broadcast:

- [`setExternalVideoSource`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7): Configures the external video source.
- [`pushVideoFrame`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985): Pushes the external video frames.

The application can encapsulate the external video frames in the `AgoraVideoFrame` format and push them to the SDK for encoding and transmitting.

This feature applies to scenarios where a user captures and renders the external video and then pushes the video frames to the SDK for transmission (the application processes the rendering).

#### 2. External audio sink (Pull Mode)

v2.3.2 adds the following methods to improve the user experience in playing the external audio:

- [`setExternalAudioSink`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638): Sets the external audio sink.
- [`pullAudioFrame`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940): Pulls the external audio frame.

The application pulls the decoded audio frames (mixed from the remote users) from the media engine for external playback.

The difference between `pullAudioFrame` and [`onPlaybackAudioFrame`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac): 

- `pullAudioFrame`: The application pulls the audio frames and:

	- Specifies the number of audio samples for playback.
	- Adjusts the frame buffer. 
	- Allows for a delay in processing the audio frame. 
	
	This method avoids problems caused by jitter in the external audio, such as an unsynchronized audio playback.
	
- `onPlaybackAudioFrame`: The SDK pushes the audio frames to the application once every 10 ms. Any delay in processing the audio frames may result in audio jitter.

#### 3. Video quality in a live broadcast

v2.3.2 adds the [minBitrate](https://docs.agora.io/en/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#ab3249ca88227aaf36f21681b9de7ced2) parameter (minimum encoding bitrate) in the [setVideoEncoderConfiguration](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) method. The SDK automatically adjusts the encoding bitrate to adapt to the network conditions. Using a value greater than the default value forces the video encoder to output high-quality images but may cause more packet loss and hence sacrifice the smoothness of the video transmission. Agora does not recommend changing this value unless you have special requirements for image quality.

#### 4. Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a5e117be71d38d813208198f4064aa964) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing. See [Adjust the Volume](../../en/Video/volume_windows.md) for the scenarios and corresponding APIs.

#### 5. Fallback options for a live broadcast under unreliable network conditions

Unreliable network conditions affect the overall quality of a live broadcast. v2.3.2 adds the [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec) and [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7) methods to allow the SDK to:

- Automatically disable the video stream when the network conditions cannot support both audio and video, or
- Enable the video when the network conditions improve. 

The SDK triggers the [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513) or [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74) callback when the stream falls back to audio-only or switches back to the video.

#### 6. Upstream and Downstream statistics of each remote user/host

v2.3.2 adds the [`onRemoteAudioTransportStats`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d) and [`onRemoteVideoTransportStats`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f)  callbacks to provide the upstream and downstream statistics of each remote user/host. During a call or live broadcast, the SDK triggers these callbacks once every two seconds after the local user receives audio/video packets from a remote user. The callbacks return the user ID, received audio/video bitrate, packet loss rate, and network time delay (ms).

#### 7. New video encoder configuration

To support video rotation scenarios and improve the quality of the custom video source, v2.3.2 deprecates the [`setVideoProfile`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb) method and replaces it with the [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) method to set the video encoder configurations. The [`VideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. You can still use the `setVideoProfil`e method, but Agora recommends using the `setVideoEncoderConfiguration` method to set the video profile.

#### 8. Virtual sound card

v2.3.2 adds the `deviceName` parameter in the [`enableLoopbackRecording`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a065f485fd23b8c24a593680a47d754aa) method, allowing you to use a virtual sound card for audio recording:

- To use the current sound card, set deviceName as NULL.
- To use a virtual card, set `deviceName` as the name of the virtual card.

### Improvements

#### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `onAudioQuality` callback and replaces it with the `onRemoteAudioStats` callback to improve the accuracy of the call quality statistics. The `onRemoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real user experience. In addition, v2.3.2 optimizes the algorithm of the `onNetworkQuality` callback for the uplink and downlink network qualities.

- [`onRemoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`onNetworkQuality`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80003ae8cce02039f3aa0e8ffad7deed): Reports the last mile network quality of each user in the channel.

Agora plans to improve the following callback in subsequent versions:

- [`onLastmileQuality`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see [Report In-call Statistics](../../en/Video/in_call_statistics_windows.md).

#### 2. New network connection policy 

v2.3.2 adds the following API method and callback to get the current network connection state and reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362): Gets the connection state of the SDK.
- [`onConnectionStateChanged`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`onConnectionInterrupted`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483) and [`onConnectionBanned`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `onConnectionStateChanged` callback when the network connection state changes. The SDK also triggers the `onConnectionInterrupted` and `onConnectionBanned` callbacks under certain circumstances, but Agora does not recommend using them.

#### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a748c30a6339ec9798daa0d1b21585411) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live broadcast. Application developers can use this feedback for future product improvement. Agora strongly recommends integrating this method in your application.


#### 4. Improves the audio device usability

v2.3.2 optimizes the audio device selection to fix the no audio issue when a user switches the audio device during a call or live broadcast.

#### 5. Other improvements

- Minimizes packet loss under unreliable network conditions in the Live Broadcast profile.
- Accelerates the video quality recovery under network congestion.
- Improves the stability in pushing streams.
- Optimizes the API calling threads.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.
- Optimizes the screen-sharing function.

### Issues Fixed

The following issues are fixed in v2.3.2:

#### SDK

- Flashes when detecting a device.

#### Audio

- A user joins a live broadcast with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another application.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset.
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an application switches back from the background. 
- Crashes in the audio module.

#### Video

- Hardware encoder issues when using an external video source on x86 devices.
- Occasional issues when using an external video source.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.
- Occasional long intervals between joining a channel and seeing the first remote video frame.
- The remote user's video is mirrored when sharing part of the desktop and when the application calls the `setVideoProfile` method to specify the video resolution.
- Blurry video when sharing a high-resolution screen.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.

### API Changes

To improve the user experience, Agora has made the following changes to the APIs:

#### Added

- [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e)
- [`setExternalVideoSource`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)
- [`pushVideoFrame`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)
- [`setExternalAudioSink`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638)
- [`pullAudioFrame`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7)
- [`getConnectionState`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734)
- [`onConnectionStateChanged`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)
- [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513)
- [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74)
- [`onRemoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)
- [`onRemoteAudioTransportStats`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d)
- [`onRemoteVideoTransportStats`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f)

#### Deprecated

- [`setVideoProfile`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb)
- [`onAudioQuality`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a36ad42975f3545382de07875016fb7fa)
- [`onConnectionInterrupted`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483)
- [`onConnectionBanned`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404)


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
-   [Video Conference of 7+ Users](../../en/Video/seventeen_people_windows.md)


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
-   Web: Added and updated a series of APIs to enable interoperability between the web browsers and native clients for communication or live broadcast scenarios. For details, refer to [Release Notes](../../en/Video/release_web_video.md).


Recording: Added real-time video mixing, web recording, and callback functions. For details, refer to [Release Notes for the Recording SDK](../../en/Product%20Overview/release_recording.md).

#### Improvements:

Android/iOS/macOS/Windows: In the communication scenario, an improvement for the 320 &times; 180 resolution profile:

- Keep the video smooth under poor network and equipment conditions.
- Enhance the image quality to be better than 180p under good network and equipment conditions.



#### Issues Fixed:

- Android: Bluetooth issues related to audio routing.
- Android/iOS/macOS/Windows: Occasional crashes.







